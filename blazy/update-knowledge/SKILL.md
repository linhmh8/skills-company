---
name: update-knowledge
description: Use when a user wants to create or refresh a single-file ecosystem skill that fills the agent's post-cutoff knowledge gap from official release notes, changelogs, migration guides, and API deltas.
---

# Update Knowledge

## Overview
Use this skill to create or refresh one dense ecosystem handbook skill that fills post-cutoff coding gaps. The output is one `SKILL.md` per ecosystem. It should read like an operational handbook for implementation, not like a release index and not like a thin summary.

## What This Skill Produces
- One ecosystem skill folder named `<ecosystem>-modern-playbook`
- One dense `SKILL.md`
- No `references/`, `README`, release index, or auxiliary docs unless the user explicitly asks for them

## Ask Only One Question If Needed
- If the ecosystem is not clear, ask exactly one direct question: which ecosystem should be updated?
- If the user already named the ecosystem, do not ask extra preference questions unless ambiguity is real.

## Source Policy
- Use official sources first:
  - changelog
  - release notes
  - migration guides
  - docs
  - package registry metadata
  - foundation or project release posts
- Prefer primary sources over tutorials or commentary.
- Live-check current stable versions and supported lines before writing if they may have changed.

## Scope Filter
Include what changes coding decisions:
- new APIs, directives, traits, utilities, components, syntax, config patterns
- breaking changes, renamed behavior, removed or discouraged patterns
- migration traps
- better modern defaults
- toolchain or runtime constraints that change implementation choices
- exact “old habit -> new habit” replacements

Compress hard or exclude:
- marketing copy
- organization news
- security advisories unless they directly change code you should write
- patch-by-patch noise with no coding consequence

## Mandatory Output Shape
Every generated ecosystem skill should stay single-file and should usually include:
- `Overview`
- `Current Snapshot`
- `Default Guidance`
- `Use Now`
- `Avoid / Don't Reach For`
- `Old Habit -> New Habit` or `Old -> New API Cheat Sheet`
- `Important Post-Cutoff Deltas`
- task-oriented sections such as `If You Are Touching X`, `Quick Recipes`, `Directive Cookbook`, or `Task Recipes`
- `Safe Defaults For Agents`
- `Live Re-check Only When Needed`

## Anti-Thinness Rules
- Do not stop at feature lists.
- Do not stop at “what changed”.
- Encode `when to use`, `when not to use`, and `caveat` for important APIs.
- Add minimal snippets or tiny before/after examples for migration-sensitive APIs.
- Add version gates when advice depends on specific versions, editions, toolchain lines, or plugins.
- Prefer task-oriented sections over release-chronology dumps.
- If a section repeats the same idea in different words, rewrite it into a decision rule instead.

## Workflow
1. Identify the ecosystem and target skill path.
2. If the skill already exists, read it first and preserve useful wording only if it is still accurate.
3. Gather official post-cutoff material. Default lower bound is the model cutoff `2024-06` unless the user asks otherwise.
4. Extract coding deltas, not release bookkeeping.
5. Convert the findings into a dense single-file handbook.
6. Ensure the handbook answers:
   - what should the agent do now?
   - what should it stop doing?
   - what old examples are now misleading?
   - what exact APIs or patterns replaced them?
7. Verify frontmatter and single-file structure.

## Richness Checklist
Before finishing, make sure the generated skill contains most of these when relevant:
- `Version Gates` or a quick gating matrix
- `Use / Avoid / Caveat` bullets for high-risk APIs
- `Old -> New` mappings
- minimal code snippets
- migration traps
- framework or toolchain edge cases
- “safe defaults” blunt enough that an agent can act without guessing

## Naming Convention
- Prefer `<ecosystem>-modern-playbook`.
- Keep the name searchable and plain.
- Do not invent clever names that hide the ecosystem keyword.

## Subagent Use
- If the ecosystem is broad or the changelog is heavy, spawn a focused subagent.
- The subagent brief should ask for:
  - top implementation gaps
  - concrete APIs/patterns to encode
  - exact sections needed to avoid agent guessing
  - primary-source links
- Integrate results back into the single local `SKILL.md`.

## Existing Ecosystem Map
- Go: `go-modern-playbook`
- Vue: `vue-modern-playbook`
- Tailwind CSS: `tailwind-modern-playbook`
- Rust: `rust-modern-playbook`
- Bevy: `bevy-modern-playbook`

## Completion Check
Before claiming the update is done, verify:
- frontmatter is valid
- the file is still a single `SKILL.md`
- the content is coding-oriented rather than release-index-oriented
- the skill gives enough decision support that an agent can code without bouncing back to docs for common post-cutoff cases
