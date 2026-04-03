---
name: vue-modern-playbook
description: Use when writing, reviewing, upgrading, or debugging Vue code and post-2024-06 release knowledge may matter, especially Vue 3.5 reactive props destructure, useTemplateRef, watcher cleanup, SSR hydration APIs, custom elements, or the current stance on Vapor Mode.
---

# Vue Modern Playbook

## Overview
Use this skill when coding in Vue and post-`2024-06` release knowledge may change what you should build. The stable target is Vue `3.5.x`. This file is task-oriented: props, refs, watchers, SSR, custom elements, and the current boundary around `3.6` / Vapor.

## Current Snapshot
- Checked: `2026-03-19`
- Model global knowledge cutoff: `2024-06`
- Baseline inference before live lookup: Vue `3.4` era
- Latest stable verified at check time: `3.5.30`
- Latest beta verified at check time: `3.6.0-beta.8`

## Default Guidance
- Treat Vue `3.5.x` as the stable coding target.
- Treat `3.6` / Vapor as opt-in beta work only.
- Prefer task-specific modern APIs where they remove ceremony:
  - reactive props destructure
  - `useTemplateRef()`
  - watcher `pause()` / `resume()`
  - `onWatcherCleanup()`
  - lazy hydration strategies for SSR async components
  - `useId()` with `app.config.idPrefix`

## Version Gates
- If the repo is `<3.5`, do not recommend reactive props destructure as the default idiom.
- If the repo is `<3.5`, do not assume lazy hydration APIs or the newer CE helpers exist.
- If `@vue/language-tools` / `vue-tsc` is `<2.1`, do not assume static `useTemplateRef()` auto-inference is as smooth as current docs imply.
- If the repo is not SSR, hydration strategies are irrelevant.
- If the repo is not using custom elements, do not load CE-specific advice into normal component work.

## Tooling Alignment
- `useTemplateRef()` is nicest when SFC tooling is current.
- Reactive props destructure is compiler behavior. If the SFC compiler/toolchain is stale, do not assume current rewriting semantics.
- SSR hydration strategies apply to async components in SSR contexts, not normal client-only components.
- Vapor is not generic “faster Vue”; it is a separate opt-in compilation mode with important constraints.

## Use Now
- In Vue `3.5+`, use reactive props destructure in `<script setup>` when it improves clarity.
- Use `useTemplateRef()` for template refs instead of `const el = ref(null)` when that is all you need.
- Use watcher handles with `pause()` / `resume()` when a watcher needs temporary suppression instead of extra boolean state.
- Use numeric `deep` when you need bounded nested watching instead of full recursive traversal.
- Use `onWatcherCleanup()` for cleanup that belongs to watcher invalidation and can be registered synchronously.
- In SSR work, use lazy hydration strategies instead of ad hoc “hydrate later somehow” logic.
- Use `useId()` and `app.config.idPrefix` for stable server/client IDs across app roots.
- For custom elements, use the `3.5` CE helpers instead of older workaround-heavy assumptions.

## Avoid / Don't Reach For
- Don't treat `3.6` / Vapor as the normal default architecture.
- Don't pass destructured props directly where a watcher or composable expects a source getter.
- Don't call `onWatcherCleanup()` after an `await`.
- Don't use `data-allow-mismatch` as a blanket way to ignore SSR bugs.
- Don't keep reaching for `ref(null)` for template refs if `useTemplateRef()` is the simpler shape.
- Don't assume older custom-element limitations still apply in `3.5+`.

## Old Habit -> New Habit
- `const props = defineProps(...); props.foo` everywhere
  - Replace with reactive props destructure when it genuinely improves readability
- `const el = ref<HTMLElement | null>(null)`
  - Replace with `useTemplateRef('el')` when the template ref is the main goal
- watcher cleanup passed around manually
  - Replace with `onWatcherCleanup()` or the watcher callback `onCleanup` argument
- SSR ID hacks
  - Replace with `useId()` and `app.config.idPrefix`
- ad hoc delayed hydration logic
  - Replace with `hydrateOnIdle`, `hydrateOnVisible`, `hydrateOnInteraction`, or `hydrateOnMediaQuery`

## Important Post-Cutoff Deltas
- `3.5`
  - reactive props destructure enabled by default
  - reactivity refactor work
  - watcher `pause()` / `resume()`
  - numeric `deep`
  - `onWatcherCleanup()`
  - lazy hydration strategies for SSR async components
  - `useId()`
  - `app.config.idPrefix`
  - `data-allow-mismatch`
  - CE helpers: `configureApp`, `useHost`, `useShadowRoot`, `this.$host`, `shadowRoot: false`, `nonce`
  - `useTemplateRef()`
  - deferred `<Teleport defer>`
  - `app.onUnmount()`
- `3.6` beta
  - Vapor is much further along
  - still not stable-by-default guidance

## If You Are Touching Props
- Reach for reactive props destructure in `3.5+` when it makes code smaller and clearer.
- Remember what changed: destructured props are compiler-rewritten to stay reactive.
- Do not treat destructured values as generic sources in APIs that want getters.
- If passing a destructured prop into `watch()` or a composable that accepts sources, use a getter:

```ts
const { foo } = defineProps<{ foo: string }>()

watch(() => foo, () => {
  /* ... */
})
```

- If you need prop defaults and destructuring reads better, destructured defaults are now a valid normal path.
- If a repo is still pre-`3.5`, fall back to the older `props.foo` mental model.

## If You Are Touching Template Refs
- Prefer `useTemplateRef()` when the job is “bind a template ref and use it”.
- In current tooling, static refs in SFCs can infer nicely; dynamic or generic component refs may still need explicit typing.

```ts
const input = useTemplateRef<HTMLInputElement>('search')
```

- Avoid keeping old `ref(null)` boilerplate unless the repo/tooling is old or the case is unusual.

## If You Are Touching Watchers
- Use `pause()` / `resume()` when toggling a watcher is cleaner than threading flags through the callback.
- Use numeric `deep` if you need a bounded traversal budget.
- Use `onWatcherCleanup()` only during synchronous watcher execution.

```ts
watch(source, async () => {
  const controller = new AbortController()
  onWatcherCleanup(() => controller.abort())
  await fetch(url, { signal: controller.signal })
})
```

- If cleanup registration must happen after async boundaries, use the callback `onCleanup` argument instead of `onWatcherCleanup()`.

## If You Are Touching SSR / Hydration
- Use lazy hydration only for SSR async components.
- Reach for:
  - `hydrateOnIdle`
  - `hydrateOnVisible`
  - `hydrateOnInteraction`
  - `hydrateOnMediaQuery`
- `hydrateOnInteraction` replays the triggering interaction after hydration, which matters for UX.
- Use `data-allow-mismatch` surgically. It can scope mismatches such as `text`, `children`, `class`, `style`, or `attribute`.
- Use `useId()` with `app.config.idPrefix` as the normal answer for stable IDs across multiple roots.

## If You Are Touching Custom Elements
- Re-check old assumptions. Vue `3.5+` materially improved the CE story.
- Use `configureApp` to customize the CE-hosted app.
- Use `useHost()` / `useShadowRoot()` or `this.$host` instead of DOM workarounds.
- Use `shadowRoot: false` only when you explicitly want light DOM behavior.
- Use `nonce` support when CSP or injected styles require it.

## Decision Rules
- If the repo is stable Vue app work:
  - target `3.5.x`
  - use stable `3.5` APIs
  - ignore Vapor unless the repo already opted in
- If the task is about props ergonomics:
  - prefer destructure in `3.5+`
  - use getters when passing values into source-based APIs
- If the task is about refs:
  - prefer `useTemplateRef()`
- If the task is about SSR mismatches:
  - fix the mismatch first
  - only then use `data-allow-mismatch` if the mismatch is intentional

## Trap Doors With Minimal Examples

```ts
// wrong
const { foo } = defineProps<{ foo: string }>()
watch(foo, () => {})

// right
watch(() => foo, () => {})
```

```ts
watch(source, async () => {
  await nextTick()
  // wrong: too late
  // onWatcherCleanup(...)
})
```

```ts
// use when the component is async + SSR rendered
const AsyncPanel = defineAsyncComponent({
  loader: () => import('./Panel.vue'),
  hydrate: hydrateOnVisible(),
})
```

## Vapor / Beta Quarantine
- Vapor is not default guidance.
- Only consider it if the repo explicitly wants that track.
- Current caveats still matter:
  - no Options API
  - no `app.config.globalProperties`
  - `getCurrentInstance()` is `null` inside Vapor components
  - Suspense is not supported in Vapor-only mode
  - directive contracts differ
  - VDOM interop still has rough edges
- Treat Vapor as “separate architecture with active instability”, not “drop-in faster compiler mode”.

## Safe Defaults For Agents
- Stable target is Vue `3.5.x`.
- Use reactive props destructure in `3.5+`, but pass getters to source-based APIs.
- Use `useTemplateRef()` for plain template-ref cases.
- Use `onWatcherCleanup()` only before any `await`.
- Use lazy hydration only in SSR async-component work.
- Do not default to Vapor.

## Live Re-check Only When Needed
- Exact latest patch behavior matters for CE, SSR, or compiler edge cases.
- The repo tooling may lag `@vue/language-tools` / `vue-tsc`.
- The task explicitly wants Vapor or `3.6` beta behavior.
