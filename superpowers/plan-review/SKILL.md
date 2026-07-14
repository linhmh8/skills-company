---
name: plan-review
description: Use when a written implementation plan needs an adversarial review in a separate session before coding, delegation, or approval, especially for non-trivial, risky, boundary-crossing, or hot-path work.
---

# Plan Review

## Overview

Use this skill to pressure-test a plan before code exists.
Treat the plan as guilty until it proves owner cleanliness, boundary clarity, proof sufficiency, and resistance to fake-pass execution.

## Use When

- a written plan already exists
- implementation has not started yet
- the slice is non-trivial, risky, boundary-crossing, or hot-path sensitive
- you want a separate-session hostile pass before coding, delegation, or approval
- the executor could take a plausible shortcut unless the plan blocks it up front

## Do Not Use When

- no written plan exists yet; use `writing-plans` first
- code is already written; use `requesting-code-review`, `nova-review`, or the repo's review path instead
- the change is tiny and mechanical enough that plan review would add ceremony without reducing risk

## Required Inputs

Provide the smallest set of artifacts that lets the reviewer attack the real risk:

- task statement
- written plan
- governing spec, doctrine, or policy docs
- intended owner seams or touched paths
- constraints, compatibility requirements, and proof expectations

If any of these are missing, say so before approval and treat that as a review limitation.

## Review Stance

- default to rejection until the plan earns approval
- attack assumptions, not prose style
- look for the weakest implementation that could still fake-pass under this plan
- prefer identifying execution risk over proposing implementation details
- keep wording nits non-blocking unless they hide real ambiguity

## What To Attack

- wrong owner seam or blurry boundary contracts
- missing or weak `Primary Contract`, `First Proof`, or `First Slice`
- hidden coupling to code or docs outside the stated slice
- omitted failure modes: error paths, rollback, partial update, stale state, ordering, concurrency, idempotency
- proof gaps: tests or checks that only prove the happy path or the chosen implementation
- test expectations without authority from a spec, accepted decision, invariant, or external oracle
- tests that silently select an unresolved owner, public API, or architecture
- RED-only units, horizontal test phases, or compile-only RED presented as behavioral proof
- leaf tests presented as complete owner, lifecycle, concurrency, or cutover proof
- internal-owner mocks, test-only state injectors, or public constructors added only to make tests possible
- fake-safe staging, shims, bridges, dual paths, or compile-only scaffolding
- hot-path traps: extra allocs, rescans, cross-products, or perf regressions disguised as architecture
- missing stop conditions or acceptance bars that let the executor improvise contracts mid-flight

## Fake-Pass Questions

Before accepting the plan, ask:

- what is the smallest patch that could satisfy the prose while missing the real contract
- what would an executor implement if they optimized for speed instead of architecture
- what proof could go green while the owner boundary is still wrong
- what staging path would leave bridge code or compatibility debt behind
- what test could force a speculative API or mechanism while claiming to prove only behavior

If the plan does not block these routes explicitly, it is not review-ready.

## Output Format

- `Verdict:` `Reject` | `Accept with Conditions` | `Accept`
- `Findings:` blocking issues ordered from highest severity to lowest, with plan section references when possible
- `Missing Proofs:` proof obligations that must be added or strengthened
- `Required Plan Changes:` concrete changes needed before implementation
- `Advisory:` optional improvements that should not block execution

Do not dilute the verdict. If the plan is underspecified on a real risk surface, reject it or gate it with explicit conditions.

## Session Model

Use this skill in the review session itself.
Do not spawn subagents from this skill.
If you only have partial context, review aggressively within that limit and say exactly what is missing.

## Codex Guidance

- expect `writing-plans` output to name `Primary Contract`, `First Proof`, and `First Slice`; flag their absence directly
- reject plans that imply horizontal "all tests first, all implementation later" workflow when tests are the proof surface
- reject plans that split `RED` and `GREEN` across execution units
- require compile-fail proof to stay scoped to a decided static contract
- require lifecycle and hard-cut claims to include integrated production-path scenarios, controlled faults, positive cutover, and deletion evidence
- reject plans whose claimed proof does not match the final claim they intend to make
- when a plan claims one-shot execution, verify that the acceptance bar really describes a one-shot end state

## NOVA Note

For `NOVA`, read repo doctrine first and attack:

- seam ownership and owner-cut cleanliness
- runtime hot-path shape
- boundary contracts and owner drift
- proof bar versus repository policy
- any plan that preserves convenience scaffolding instead of the final owner-clean shape

## Bottom Line

Good plan review is falsification, not encouragement.
If the plan can still hide a weak implementation, the review is not done.
