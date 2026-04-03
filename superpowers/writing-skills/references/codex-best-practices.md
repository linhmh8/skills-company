# Codex Skill Authoring Patterns

Use this reference when you want compact, Codex-native guidance for writing or rewriting skills.

## Core Ideas

- optimize for trigger quality before rule strictness
- keep workflow proportional to task risk
- remove environment-specific assumptions that are not true in Codex
- make proof surfaces explicit
- give strong models room to think, but not room to drift

## Discovery

Descriptions should say when to use the skill, not summarize its whole process.
Good descriptions improve routing.
Over-detailed descriptions create shortcuts that stop the agent from reading the skill body.

## Scope Control

Every skill should include at least one negative trigger.
If you cannot say when the skill should stay out, it will overfire.

Good examples:

- "Skip for tiny mechanical edits."
- "Do not require new wrapper tests when boundary proof is stronger."
- "Use only when behavior or contract is changing."

## Proof Surfaces

Skills should name the evidence that matches the claim:

- bug fix -> failing repro or regression proof
- new behavior -> failing test or contract proof
- refactor -> characterization or invariant proof
- owner cut -> boundary inspection and owner proof
- hot path -> benchmark or validator artifact
- mechanical/doc change -> targeted validation only

## Environment Discipline

Write for actual Codex tools and patterns:

- local file reads instead of imaginary skill tools
- bounded subagent scopes instead of roleplay workflows
- apply_patch for file edits when manual editing is required
- repo doctrine over chat memory when working in governed codebases

## Structure

Most effective skills are short:

- Overview
- When to Use
- Workflow
- Failure Modes
- Bottom Line

Push bulky details into `references/` only when those details change outcomes.

## Anti-Patterns

- absolute rules with no negative trigger
- process that exists only to sound disciplined
- instructions copied from a different agent platform
- tests or reviews required for every edit regardless of risk
- proof that checks labels, counters, or wrappers instead of the real delta
- source-grep tests added only to prove a shim, helper, or transition name disappeared
- wording-lock tests that fail on harmless renames while behavior stays correct

## Review Questions

Before keeping a skill change, ask:

- does this change a real decision
- does it reduce drift or just add ceremony
- does it match Codex tool reality
- does it make the right proof surface more likely

## Bottom Line

Good Codex skills are specific, bounded, and evidence-aware.
They do not try to win by sounding stricter than the problem requires.
