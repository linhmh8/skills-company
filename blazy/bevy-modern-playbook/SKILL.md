---
name: bevy-modern-playbook
description: Use when writing, reviewing, upgrading, or debugging Bevy code and post-2024-06 release knowledge may matter, especially Required Components, relationships, Message vs observer Event, rendering architecture changes, UI/text expansion, asset reader changes, or Bevy minor-line migrations.
---

# Bevy Modern Playbook

## Overview
Use this skill when coding in Bevy and the work might be shaped by post-`2024-06` changes. Bevy minors should be treated like practical majors. This file is meant to tell the agent what to write now, what old API instincts to drop, and what migration traps to expect between `0.14` and `0.18`.

## Current Snapshot
- Checked: `2026-03-19`
- Model global knowledge cutoff: `2024-06`
- Baseline inference before live lookup: Bevy `0.13`
- Latest stable verified at check time: `0.18.1`

## Default Guidance
- Treat Bevy `0.18.x` as the current stable target unless the repo is pinned lower.
- Treat each Bevy minor bump as migration work, not a casual dependency bump.
- Expect these modern idioms:
  - `Required Components`
  - relationships via `ChildOf` / `Children`
  - buffered `Message`s vs observer `Event`s
  - retained render world mental model
  - `RenderTarget` as a component
  - richer UI/navigation/text surface

## When To Use / When Not To Use
- Use `Required Components` when composition should automatically bring required pieces along.
- Use buffered `Message`s when you need queued, buffered communication.
- Use observer events when you want targeted reactive event handling.
- Use relationships for hierarchy thinking instead of old parent-child intuition.
- Avoid writing new code as if bundles are still the obvious composition center.
- Avoid treating “events” as one unified concept; `Message` and observer `Event` now mean different things.

## Use Now
- Spawn core gameplay/render entities with the newer component-first mindset.
- Use `#[require(...)]` and required-component registration where composition demands it.
- Use `ChildOf` / `Children` relationships and newer hierarchy helpers.
- Use `MessageWriter` / `MessageReader` for buffered flow.
- Use observer events with `On<...>` when reactivity/targeting is the point.
- Use `RenderTarget` as its own component instead of writing older camera-target code.
- Use first-party camera controllers and modern UI helpers when they fit instead of rebuilding trivial scaffolding.

## Avoid / Don't Reach For
- Don't keep defaulting to old built-in bundles as the normal composition path.
- Don't keep using `EventWriter` / `EventReader` names in newer code when the repo is already on the newer message model.
- Don't think hierarchy is only old parent/child APIs.
- Don't set `Camera.target` in newer code where `RenderTarget` is the intended model.
- Don't assume asset readers or custom asset sources still use older reader expectations.

## Old -> New API Cheat Sheet
- built-in bundles as primary composition
  - Prefer `Required Components`
- old parent/child intuition
  - Prefer relationships with `ChildOf` / `Children`
- `EventWriter<T>` / `EventReader<T>`
  - Prefer `MessageWriter<T>` / `MessageReader<T>` for buffered messaging
- `Trigger`
  - Prefer `On`
- old camera target field assumptions
  - Prefer `RenderTarget` component
- `OnAdd`
  - `Add`

## Important Post-Cutoff Deltas
- `0.14`
  - already beyond the cutoff baseline, but now old in modern Bevy terms
- `0.15`
  - `Required Components`
  - built-in bundle deprecation pressure
  - `app.register_required_components`
  - `Handle<T>` no longer a component
- `0.16`
  - relationships with `ChildOf` / `Children`
  - improved hierarchy spawning
  - `Query::single()` returns `Result`
  - GPU-driven rendering work and more render shifts
- `0.17`
  - `Message` vs observer `Event`
  - `EventWriter` / `EventReader` rename pressure
  - `Trigger` -> `On`
  - `#[derive(EntityEvent)]`
  - `Commands::write_message`
  - more render API decoupling
- `0.18`
  - immutable entity events
  - `RenderTarget` as component
  - `Reader::seekable()` / `SeekableReader`
  - cargo feature collections like `ui`, `2d`, `3d`
  - `FreeCamera` / `PanCamera`
  - `AutoDirectionalNavigation`
  - `FontFeatures`
  - pickable text sections
  - `IgnoreScroll`
  - `TryStableInterpolate`
  - `remove_systems_in_set`

## Core Mental Shifts
- Component composition moved further away from bundle-centric thinking.
- Hierarchy is relationship-driven now.
- Events split conceptually:
  - buffered messages
  - observer/entity events
- Render architecture and render-world boundaries changed enough that old examples are often misleading.

## Task Recipes

### Spawn With Required Components
```rust
#[derive(Component)]
#[require(Transform, Visibility)]
struct Player;

commands.spawn(Player);
```

### Hierarchy / Relationships
```rust
let parent = commands.spawn(Name::new("Parent")).id();
commands.spawn((Name::new("Child"), ChildOf(parent)));
```

### Buffered Message
```rust
commands.write_message(MyMessage { value: 1 });
```

### Observer Event
```rust
app.add_observer(|trigger: On<MyEntityEvent>, mut commands: Commands| {
    let entity = trigger.entity();
    commands.entity(entity).despawn();
});
```

### Camera Render Target
```rust
commands.spawn((
    Camera3d::default(),
    RenderTarget::Image(image_handle),
));
```

## Do This, Not That
- Use `Required Components` for dependency-like composition.
  - Do not keep stuffing every required piece into custom bundles by reflex.
- Use relationships for hierarchy.
  - Do not keep writing hierarchy code as if old parent-child APIs are the whole story.
- Use buffered `Message`s for queued communication.
  - Do not use observer events when you really want buffered transport semantics.
- Use observer events for reactive targeting.
  - Do not treat them as just renamed messages.
- Use `RenderTarget` as a component.
  - Do not copy old camera-target patterns from pre-`0.18` examples.

## Minor-Version Breaking Changes
- `0.15`
  - required-component model lands
  - bundle assumptions weaken
  - `Handle<T>` stops being a component
- `0.16`
  - relationships change hierarchy code shape
  - query helpers change signatures
- `0.17`
  - messaging/event naming and API model changes
  - system-set and lifecycle names shift
- `0.18`
  - render target modeling changes
  - asset readers and UI/navigation surface expand again

## Common Migration Errors / Search Terms
- “`Handle<Image>` is not a component”
- “`EventWriter` / `EventReader` no longer match current docs”
- “`Trigger` not found”
- “`Query::single` return type changed”
- “camera target field no longer matches examples”
- “custom asset reader trait or seekable reader mismatch”
- “old system set removal helper names changed”

## UI / Tooling Surface That Grew A Lot
- standard widgets
- headless widgets
- directional navigation
- `AutoDirectionalNavigation`
- pickable text sections
- `FontFeatures`
- UI gradients
- first-party camera helpers like `FreeCamera` and `PanCamera`

## Rendering / Asset Surface That Changed Developer Choices
- retained render world changed app-entity vs render-entity mental models
- `RenderTarget` becoming a component changed camera output composition
- asset readers gained more capability again with seekable-reader support
- custom asset sources now need a more explicit reader story

## Safe Defaults For Agents
- Treat the repo's exact Bevy minor as a real compatibility boundary.
- Prefer component-first composition and `Required Components`.
- Prefer relationships over older hierarchy instincts.
- Distinguish buffered `Message`s from observer events.
- Use `RenderTarget` component patterns in modern render code.
- Expect migration-guide lookup for source-target version jumps.

## Live Re-check Only When Needed
- Exact source and target minor versions matter.
- The task is a migration across multiple Bevy minors.
- Rendering, asset IO, or UI behavior depends on exact patch-level APIs.
