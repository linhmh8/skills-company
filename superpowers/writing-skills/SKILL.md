---
name: writing-skills
description: Use when creating, rewriting, or auditing skills so they align with Codex discovery, workflow, and tool reality.
---

# Writing Skills

## Overview

Good skills change real decisions with minimal context cost.
They should improve routing, proof selection, and task execution without manufacturing ceremony.

## Use This Skill When

- creating a new skill
- rewriting an outdated skill
- porting a skill from another agent environment to Codex
- reducing over-triggering or context bloat
- validating that a skill changes behavior in the intended way

## Skill Anatomy

Default structure:

- `SKILL.md` for trigger and workflow
- `agents/openai.yaml` when UI metadata should stay in sync with the skill
- `references/` for on-demand docs, examples, and detailed guidance
- `scripts/` for deterministic or repeated operations
- `assets/` for output resources

Do not add extra docs like README or changelog files just to explain the skill.

## Workflow

1. Capture the concrete failure mode.
2. Identify whether the problem is trigger, workflow, proof, or environment.
3. Define the desired behavior and non-goals.
4. Edit the smallest set of files that fixes that gap.
5. Validate with representative positive and negative scenarios.
6. Keep only the details that materially change agent behavior.

## Authoring Rules

- description should describe when the skill applies
- keep the SKILL body concise and procedural
- use references for bulky details, examples, and variants
- prefer evidence-first and risk-weighted guidance over universal rituals
- remove stale tool names and platform assumptions
- include at least one negative trigger for non-trivial skills
- when a reusable skill should appear cleanly in UI lists or chips, add or validate `agents/openai.yaml`

## References

Read these only when they are relevant:

- [references/codex-best-practices.md](references/codex-best-practices.md) for compact Codex-native design patterns
- [references/testing-skills-with-subagents.md](references/testing-skills-with-subagents.md) for scenario-based validation
- [references/trigger-and-constraint-design.md](references/trigger-and-constraint-design.md) for tightening trigger wording and stop conditions
- [references/skill-discovery-and-trigger-testing.md](references/skill-discovery-and-trigger-testing.md) for concrete routing and overfire checks

## Anti-Patterns

- descriptions that summarize workflow instead of trigger conditions
- copying process from another agent platform
- mandatory design, TDD, or review loops for trivial edits
- proof that checks wrappers, labels, or counters instead of the real delta
- reference files that are not linked from `SKILL.md`

## Bottom Line

Write skills the way you would write a good operator guide:
short, explicit, bounded, and aligned with Codex tool reality.
