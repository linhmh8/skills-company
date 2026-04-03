---
name: tailwind-modern-playbook
description: Use when writing, reviewing, upgrading, or debugging Tailwind CSS code and post-2024-06 release knowledge may matter, especially v4 migration, CSS-first configuration, package splits, theme variables, directives, scanner behavior, logical-property utilities, or modern Tailwind workflow changes.
---

# Tailwind Modern Playbook

## Overview
Use this skill when coding in Tailwind CSS and the repo may be post-`2024-06` knowledge-sensitive. Tailwind `4.x` is a real workflow shift, not just “more classes”. This file is meant to stop guesswork in actual implementation: setup choice, directives, theme variables, scanner behavior, migration hazards, and what to stop doing.

## Current Snapshot
- Checked: `2026-03-19`
- Model global knowledge cutoff: `2024-06`
- Baseline inference before live lookup: Tailwind CSS `3.4`
- Latest stable verified at check time: `4.2.2`

## Core Mental Shift
- The real break is `3.x -> 4.x`.
- Tailwind `4.x` is CSS-first.
- Stop treating `tailwind.config.js` plus `content` arrays as the universal center of gravity.
- On `4.x`, think in this order:
  - CSS entrypoint
  - theme variables
  - source detection
  - directives and utilities
  - tool-specific package

## Default Guidance For New v4 Work
- Use `@import "tailwindcss"` as the modern entrypoint.
- Pick the package that matches the toolchain:
  - `@tailwindcss/vite`
  - `@tailwindcss/postcss`
  - `@tailwindcss/cli`
  - `@tailwindcss/webpack`
- Prefer CSS-first theme customization with `@theme`.
- Prefer built-in utilities before custom CSS or plugins.
- Use automatic source detection first.
- Reach for `@source` only when the defaults miss real code.
- Use `@utility` for custom utilities instead of old “custom class in `@layer` should behave like a utility” habits.

## Only When Migrating From v3
- Keep JS config only when the repo genuinely still needs it.
- Use `@config` to pull in JS config intentionally.
- Treat old plugin/content-array assumptions as migration baggage, not new-work defaults.
- If the repo must support older browsers, staying on `3.4.x` may still be the correct answer.

## Use Now
- Use CSS theme variables instead of over-centralizing all customization in JS.
- Use `@theme`, `@theme inline`, and `@theme static` to control token authoring and output behavior.
- Use `@source` / `@source not` / `@source inline()` to fix scanner blind spots.
- Use `@reference` in CSS modules or framework-scoped style blocks where `@apply` or `@variant` need access to theme definitions without duplicating CSS.
- Use newer v4.1 and v4.2 utilities when they reduce custom CSS:
  - `text-shadow-*`
  - `mask-*`
  - `wrap-anywhere`
  - `wrap-break-word`
  - logical block/inline sizing
  - logical inset/border/spacing utilities
  - `font-features-*`

## Avoid / Don't Reach For
- Don't build new `4.x` work around a required `content` array mindset.
- Don't assume dynamic class fragments are detected. Tailwind still wants complete class strings in source.
- Don't assume JS config is the primary knob for all customization.
- Don't keep using old PostCSS package assumptions. `@tailwindcss/postcss` is now the integration package.
- Don't assume `@tailwind` directives still exist in `v4`.
- Don't assume `hover:` always behaves on touch the way older mental models implied.
- Don't say “`start-*` and `end-*` are gone” broadly. The `4.2` shift is narrower; logical inset utilities specifically moved toward `inset-s-*` / `inset-e-*`.

## Important Post-Cutoff Deltas
- `4.0`
  - CSS-first configuration
  - automatic source detection
  - theme tokens as CSS variables
  - package split for tooling
  - container queries in core
  - modern directive model
- `4.1`
  - `text-shadow-*`
  - `mask-*`
  - wrapping and pointer utilities
  - `@source inline()`
- `4.2`
  - logical block/inline sizing and positioning surface
  - `font-features-*`
  - first-party webpack plugin
  - more default palettes

## Quick Recipes

### Vite
```ts
// vite.config.ts
import { defineConfig } from 'vite'
import tailwindcss from '@tailwindcss/vite'

export default defineConfig({
  plugins: [tailwindcss()],
})
```

### PostCSS
```js
// postcss.config.js
export default {
  plugins: {
    '@tailwindcss/postcss': {},
  },
}
```

### CSS Entry
```css
@import "tailwindcss";

@theme {
  --color-brand-500: oklch(0.65 0.17 250);
}
```

### CLI
```bash
npx @tailwindcss/cli -i ./src/input.css -o ./dist/output.css --watch
```

## Directive Cookbook
- `@import "tailwindcss"`
  - Use as the normal entrypoint.
  - Common mistake: trying to keep old `@tailwind base; @tailwind components; @tailwind utilities;`.
- `@theme`
  - Use to define or override theme variables in CSS.
  - Common mistake: thinking theme extension must live in JS config.
- `@theme inline`
  - Use when values should inline references instead of relying on later variable resolution.
- `@theme static`
  - Use when you want variables emitted even if utilities are not detected in current source.
- `@config`
  - Use when the repo still needs JS config in a `v4` world.
  - Common mistake: assuming JS config remains the primary shape for all new work.
- `@source`
  - Use when auto detection misses source files or external packages.
  - Common mistake: reaching for it before checking that classes are written as static strings.
- `@source not`
  - Use to exclude noisy paths from detection.
- `@source inline()`
  - Use to safelist known utilities in CSS.
- `@source not inline()`
  - Use to explicitly block inline-detected candidates.
- `@reference`
  - Use in CSS modules or Vue/Svelte/Astro style blocks so `@apply` and `@variant` can see theme definitions without duplicating CSS output.
- `@utility`
  - Use for custom utilities.
  - Common mistake: porting old `@layer` habits unchanged.
- `@variant`
  - Use when authoring variant-aware rules in CSS.
- `@custom-variant`
  - Use to redefine or introduce variant behavior such as custom `hover`.
- `@plugin`
  - Use only when a plugin is still necessary after checking whether core now covers the need.
- `--alpha()`
  - Use for alpha-adjusted colors in CSS-driven authoring.
- `--spacing()`
  - Use for spacing math derived from the theme scale.

## Theme Variable Patterns
- Extend a namespace:

```css
@theme {
  --color-brand-500: oklch(0.65 0.17 250);
}
```

- Replace a namespace:

```css
@theme {
  --color-*: initial;
  --color-brand-500: oklch(0.65 0.17 250);
}
```

- Replace everything:

```css
@theme {
  --*: initial;
  --spacing: 4px;
  --font-sans: "Satoshi", sans-serif;
}
```

- Use `@theme inline` when variable references should resolve inline instead of via later variable lookup.
- Use `@theme static` when a shared theme package should always emit tokens even before local extraction sees matching utilities.

## Scanner / Extraction Debugging
- Symptom: classes are missing.
  - First check whether classes are complete strings in source.
  - Do not interpolate fragments like ``bg-${color}-500``.
  - Map props to full class strings instead.
- Symptom: monorepo package classes are missing.
  - Add `@source "../packages/ui"` or use `source("../src")` style entrypoint control.
- Symptom: too much noise is being scanned.
  - Use `source(none)` at import time or `@source not` to narrow scope.
- Symptom: external UI library classes are missed.
  - Add explicit `@source "../node_modules/<pkg>"`.
- Symptom: scoped framework style blocks cannot `@apply`.
  - Use `@reference`.
- Default exclusions like `.gitignore`d paths and `node_modules` still matter; do not assume everything is scanned automatically.

## Framework Edge Cases
- Vue, Svelte, and Astro `<style>` blocks often need `@reference` to see theme variables and utilities for `@apply`.
- CSS modules usually need `@reference` for the same reason.
- Multiple stylesheet entrypoints can confuse mental models around detection; ensure the right stylesheet owns the import and source rules.

## Tailwind 4.0: What Actually Changed
- New high-performance engine.
- CSS-first configuration and customization.
- Automatic source detection replacing the old required `content`-array mindset.
- Built-in import support.
- Theme tokens exposed as CSS variables.
- Container queries moved into core.
- First-party Vite integration.

## Tailwind 4.0: Package / Tooling Changes
- PostCSS integration moved to `@tailwindcss/postcss`.
- CLI moved to `@tailwindcss/cli`.
- Vite integration moved to `@tailwindcss/vite`.
- Webpack gained first-party support in `4.2` via `@tailwindcss/webpack`.
- Old install guides are often wrong for `v4`.

## Tailwind 4.0: Renames / Semantics To Remember
- `outline-none` became `outline-hidden`.
- Bare `ring` now means `1px` and uses `currentColor`.
- `ring-3` is the closer replacement for the old default-width expectation.
- Default `border` and `divide` color behavior shifted toward `currentColor`.
- Important modifier syntax now prefers suffix `!`.
- Variant stacking reads left-to-right in `v4`.
- Prefixes are configured with `prefix(tw)` and behave more variant-like than older prefix intuition.
- Some arbitrary-value syntax changed:
  - `bg-(--brand-color)` instead of `bg-[--brand-color]`
  - underscores can replace space-like comma hacks in some arbitrary values

## Tailwind 4.1: Useful New Surface Area
- `text-shadow-*`
- text-shadow color and opacity forms
- `mask-*`
- colored `drop-shadow-*`
- `wrap-anywhere`
- `wrap-break-word`
- `pointer-*`
- `any-pointer-*`
- `self-baseline-last`
- `items-baseline-last`
- safe alignment variants
- `noscript`
- `user-valid`
- `inverted-colors`

## Tailwind 4.2: Useful New Surface Area
- New palettes such as:
  - `mauve`
  - `olive`
  - `mist`
  - `taupe`
- New logical utilities:
  - `inline-*`
  - `min-inline-*`
  - `max-inline-*`
  - `block-*`
  - `min-block-*`
  - `max-block-*`
  - `pbs-*`
  - `pbe-*`
  - `mbs-*`
  - `mbe-*`
  - `border-bs-*`
  - `border-be-*`
  - `inset-s-*`
  - `inset-e-*`
  - `inset-bs-*`
  - `inset-be-*`
- `font-features-*`
- Use the newer logical inset utilities for inset positioning.
- Do not overgeneralize this into “all start/end utilities disappeared”.

## v3 -> v4 Migration Hazards
- Old:
  - `@tailwind` directives
  - New:
  - `@import "tailwindcss"`
- Old:
  - PostCSS plugin from the main package
  - New:
  - `@tailwindcss/postcss`
- Old:
  - JS config as primary home of all theme logic
  - New:
  - CSS-first `@theme`, with JS config only where still needed
- Old:
  - `content` array is mandatory
  - New:
  - automatic detection first, then `@source`
- Old:
  - `outline-none`
  - New:
  - `outline-hidden`
- Old:
  - prefix `!important` at the front
  - New:
  - suffix `!`

## Browser / Platform Reality
- Tailwind `4.x` targets modern browsers.
- Official floor called out around `4.0`:
  - Safari `16.4+`
  - Chrome `111+`
  - Firefox `128+`
- Tailwind `4.x` is not designed around Sass/Less/Stylus-centric workflows.

## Safe Defaults For Agents
- Default to Tailwind `4.x` for new work.
- Pick the package that matches the build tool, not the old monolith assumption.
- Use `@theme` for design tokens.
- Use static class strings, not interpolated fragments.
- Use `@source` only after checking extraction assumptions.
- Use `@reference` in scoped style contexts.
- Treat `3.x` compatibility work as migration mode, not new-work mode.

## Live Re-check Only When Needed
- Exact patch-level scanner or dev-server behavior matters.
- The build toolchain is unusual or very new.
- The repo is stuck on `3.x`.
- The issue smells like extraction, monorepo pathing, or framework-style-block integration.
