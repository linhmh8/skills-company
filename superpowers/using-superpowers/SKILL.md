---
name: using-superpowers
description: Use when deciding whether skills should shape the current task and which minimal set of skills is worth loading before substantial work.
---

# Using Superpowers

## Overview

Skills are leverage, not a tax.
Load the smallest set that materially improves the task.

## Rule

Before substantial work, ask:

- is there a repo-router skill for this codebase
- is there a process skill for this problem shape
- is there a domain skill that changes the technical answer

If yes, load it.
If not, do the work directly.
Do not load a process skill just because the task mentions "refactor", "tests", or "cleanup".
Load it only when the proof surface or workflow would materially change the decision.

## Priority Order

1. repo-specific router skill
2. problem-shape skill such as debugging, planning, or review
3. domain or ecosystem skill
4. completion skill such as verification or closeout

Do not pile on five skills when one or two will do.

## Safe To Skip Skills For

- casual questions
- pure summarization or translation
- tiny obvious edits with established local patterns
- local exploration that does not change behavior or decisions yet
- doc-only, wording-only, and mechanical rename edits
- thin shim cleanup or helper extraction where behavior is unchanged and existing boundary proof is already strong
- temporary compile-keeping bridge code in an intermediate refactor task when long-lived proof already exists elsewhere

## Codex Note

In Codex, load a skill by reading its `SKILL.md` and only the extra files you actually need.
Do not rely on tool names or workflow assumptions copied from other environments.

## Misuse Patterns

- loading every vaguely related skill
- following a skill blindly when the environment has different tools
- forcing a full design or TDD ritual onto a trivial mechanical change
- loading TDD for unchanged-behavior refactors just because files moved
- adding wrapper or source-grep tests only to satisfy a process skill
- treating shim removal, adapter cleanup, or wording cleanup as if they need new unit tests by default
- treating intermediate compile-keeping bridge code as if it deserves long-lived unit tests by default
- keeping an outdated skill instead of fixing it

## Conflict Resolution

When instructions conflict:

1. explicit user instruction
2. repo doctrine or repo policy
3. the most directly relevant skill
4. general skills

## Bottom Line

Use skills to remove ambiguity and failure modes.
Do not use them to manufacture ceremony.
