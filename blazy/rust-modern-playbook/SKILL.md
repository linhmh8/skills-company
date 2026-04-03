---
name: rust-modern-playbook
description: Use when writing, reviewing, upgrading, or debugging Rust code and post-2024-06 release knowledge may matter, especially Rust 2024, async closures, trait-object upcasting, newer std APIs, Cargo resolver or MSRV behavior, or target/toolchain changes after the cutoff.
---

# Rust Modern Playbook

## Overview
Use this skill when coding in Rust and post-`2024-06` release knowledge could change what you should write. The biggest practical shift is Rust `2024`, but many daily wins are also in newer std APIs and Cargo behavior. This file is meant to reduce guessing, especially around edition gates, Cargo resolver behavior, and migration traps.

## Current Snapshot
- Checked: `2026-03-19`
- Model global knowledge cutoff: `2024-06`
- Baseline inference before live lookup: Rust `1.79`
- Latest stable verified at check time: `1.94.0`

## Default Guidance
- Treat Rust `2024` as the major post-cutoff shift, but only if the repo edition actually moved.
- Prefer stable new APIs that simplify code:
  - async closures
  - trait-object upcasting
  - `LazyCell` / `LazyLock`
  - `fs::exists`
  - `get_disjoint_mut`
  - `File::lock*`
  - `Vec::into_raw_parts`
  - `String::into_raw_parts`
  - `[T]::as_array`
  - `array_windows`
- Treat edition-sensitive guidance and Cargo resolver behavior as first-class constraints, not footnotes.

## Quick Gating Matrix
- async closures
  - Stable in: `1.85`
  - Requires edition 2024: no
  - Reach for it when: async callbacks need borrowed captures across `await`
- Rust 2024 edition
  - Stable in: `1.85`
  - Requires edition 2024: yes
  - Reach for it when: the repo intentionally migrates edition
- resolver v3
  - Stable in: `1.84`
  - Requires edition 2024: default there
  - Reach for it when: the workspace wants Rust-version-aware dependency selection
- trait-object upcasting
  - Stable in: post-cutoff stable line
  - Requires edition 2024: no
  - Reach for it when: a broader trait object should coerce to a narrower supertrait object
- `LazyLock`
  - Stable in: post-cutoff stable line
  - Requires edition 2024: no
  - Reach for it when: a global or shared lazy-initialized value beats manual `OnceLock` glue

## Use Now
- Use async closures when the old `|...| async move { ... }` pattern fights borrowing or higher-ranked callback signatures.
- Use `LazyCell` / `LazyLock` instead of hand-rolled lazy statics or unnecessary `OnceLock` wrappers.
- Use `fs::exists` when you want a direct existence check and do not need richer metadata.
- Use `get_disjoint_mut` instead of unsafe or awkward split logic when mutably borrowing multiple distinct elements.
- Use `File::lock` / `try_lock` instead of platform-specific file-locking crates if std now covers the need.
- Use resolver v3 and a correct `rust-version` when the repo is on newer Cargo and wants dependency resolution aligned to MSRV.

## Avoid / Don't Reach For
- Don't apply Rust `2024` guidance blindly to a `2021` repo.
- Don't describe Rust `2024` as syntax-only churn. Semantic changes matter.
- Don't flip to resolver v3 without understanding lockfile and CI selection changes.
- Don't migrate to new edition behavior without checking macros, unsafe boundaries, and name collisions.
- Don't assume async closures are only style sugar. They change what callback APIs can express.

## Old Pattern -> Prefer This
- `|| async move { ... }` where borrowing gets awkward
  - Prefer `async |...| { ... }`
- manual lazy initialization glue
  - Prefer `LazyCell` / `LazyLock`
- platform-specific file lock workaround
  - Prefer `File::lock*` if std now covers the use case
- resolver assumptions that ignore `rust-version`
  - Prefer explicit `rust-version` plus resolver v3-aware reasoning

## Important Post-Cutoff Deltas
- `1.80` to `1.83`
  - steady std/API stabilization after cutoff
- `1.84`
  - strict-provenance pointer APIs
  - resolver v3 is available
  - trait-solver/coherence changes can surface overlap errors
- `1.85`
  - Rust `2024` stable
  - async closures stable
  - prelude changed with `Future` / `IntoFuture`
  - some std APIs became unsafe in `2024`
- later stable line through `1.94`
  - more std stabilization
  - tooling, Cargo, and target behavior continued to move

## High-Impact Deltas By Topic

### Edition / Semantics
- Rust `2024` changed RPIT lifetime capture defaults.
- `if let` temporary scope changed.
- tail-expression temporary scope changed.
- never-type fallback changed toward `!`.
- `gen` is a reserved keyword in `2024`.

### Unsafe Boundary Changes
- `unsafe extern` is now part of the modern edition-aware model.
- Unsafe attributes need explicit `#[unsafe(...)]` for items like:
  - `no_mangle`
  - `export_name`
  - `link_section`
- `unsafe_op_in_unsafe_fn` warns by default.
- `std::env::set_var`, `remove_var`, and `CommandExt::before_exec` became unsafe in the `2024` model.

### Macro / Name Resolution
- `macro_rules!` `expr` matches more in `2024`.
- Use `expr_2021` if preserving older matching behavior matters.
- Prelude additions can create method-name collisions because `Future` and `IntoFuture` are now present.

### Cargo / Build
- Resolver v3 changes dependency selection behavior around `rust-version`.
- Automatic cache GC changed the old “Cargo cache grows forever” assumption.
- `build.build-dir` gives stronger control over build output layout.
- Config `include` makes shared Cargo config composition cleaner.
- TOML `1.1` support changes what manifests and tools may expect.

## Rust 2024 Migration Checklist
- Check whether the repo edition actually changed to `2024`.
- Review macros that rely on old `expr` matching behavior.
- Review APIs returning RPIT where capture precision matters.
- Review `if let` and tail-expression temporary lifetime assumptions.
- Review unsafe attributes and `unsafe extern`.
- Review any use of `set_var`, `remove_var`, or `before_exec`.
- Review identifiers named `gen`.
- Review method-name collisions caused by prelude additions.
- Run edition migration tooling, then inspect the result manually.

## Tiny Examples

```rust
// older shape
let cb = |req: &Request| async move {
    handle(req).await
};

// modern shape
let cb = async |req: &Request| {
    handle(req).await
};
```

```rust
// preserve old macro behavior in 2024 when needed
macro_rules! take_expr_2021 {
    ($e:expr_2021) => {
        $e
    };
}
```

```rust
use std::sync::LazyLock;

static CONFIG: LazyLock<Config> = LazyLock::new(load_config);
```

## APIs / Features Worth Reaching For
- async closures
- trait-object upcasting
- `let_chains`
- `repr(128)`
- `LazyCell`
- `LazyLock`
- `fs::exists`
- `get_disjoint_mut`
- `File::lock*`
- `Vec::into_raw_parts`
- `String::into_raw_parts`
- `[T]::as_array`
- `array_windows`
- strict-provenance pointer helpers such as `with_addr`, `map_addr`, and `without_provenance`

## Cargo / Build-System Deltas That Matter
- If the workspace uses edition `2024` or resolver `3`, Cargo prefers Rust-version-compatible dependency releases by default.
- That can change selected crate versions and CI lockfile behavior.
- `rust-version` matters more now because Cargo actually uses it more aggressively in resolution.
- Automatic cache GC means CI/local cache behavior may differ from old assumptions.
- Build output configuration got richer; do not assume the old `target/` layout is the only sane path.

## Footguns / Anti-Confusion
- Async closures are not just prettier syntax. They matter when borrowed captures cross `await`.
- Resolver v3 is not just a config detail. It can change resolved dependency versions.
- Rust `2024` migration is not done just because the code compiles after `cargo fix`.
- Prelude additions can break method resolution unexpectedly.
- Trait-solver/coherence changes can surface errors in code that previously compiled.

## Safe Defaults For Agents
- Respect the repo edition before using edition-specific guidance.
- Use async closures in new callback-heavy async code on current stable.
- Use `LazyLock` or `LazyCell` instead of hand-rolled lazy patterns.
- Treat resolver v3 plus `rust-version` as normal reasoning in modern Cargo work.
- Treat Rust `2024` migration as a checklist, not a blind formatter pass.

## Live Re-check Only When Needed
- Exact stabilization version matters.
- The repo MSRV is older than the current stable line.
- The task depends on exact target/toolchain support.
- The change touches edition migration edges, Cargo resolution, or lint behavior.
