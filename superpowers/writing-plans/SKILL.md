---
name: writing-plans
description: Use when non-trivial work needs an agent-executable plan optimized for AI-driven delivery, owner-clean one-shot slices, and explicit proof before coding or delegation.
---

# Writing Plans

## Overview

Write plans for agents, not for hesitant humans.
Assume a strong AI executor can land a broad, owner-clean slice in one pass when the target is specified clearly enough.
Optimize for end-state architecture, seam ownership, proof, blast-radius control, and hard stop conditions.
Do not decompose work into ceremony, compile-bridging, shim preservation, or fake-safe staging.

## Use When

- the work is non-trivial, multi-file, or crosses a meaningful boundary
- the blast radius is broad enough that intent and proof must be locked before edits
- delegation or handoff is likely
- repo policy requires a plan, design gate, or explicit proof ownership
- an executor could easily take a bad shortcut unless the end-state is nailed down first

Skip a formal plan for tiny local changes that a strong executor can complete safely in one pass without ambiguity.

## Core Stance

- Default to the largest coherent one-shot slice that preserves seam ownership and a clean end state.
- Split work only on durable boundaries: owner cuts, independent proof surfaces, or truly separate write scopes.
- Do not split work just to keep the tree compiling between intermediate states.
- Do not manufacture "prep", "wire-up", "follow-up cleanup", "shim now remove later", or "make it compile first" tasks unless they have durable standalone value.
- Temporary bridges, shims, compatibility facades, dual-path staging, and compile-only scaffolding are forbidden by default.
- If a bridge is truly unavoidable, the plan must treat it as debt with an explicit kill point, proof burden, and acceptance gate that removes it or records the drift.
- Treat TDD as one selectable evidence surface, not the default sequencing law.

## Lock Contract And Proof Before Coding

Before a plan is approved, pin these explicitly:

- the owner boundary or public interface that the executor must preserve or change
- the first observable contract or invariant the work must prove
- the authority for the expected result: spec, accepted decision, invariant, or external oracle
- the strongest first proof artifact for that contract
- what will intentionally remain unproven or deferred in the first slice
- any open question that could invert the architecture if answered differently

If the plan cannot name these, it is not ready for execution.

## Plan Header

Every non-trivial plan should start by locking the real target:

```markdown
# [Feature Or Slice] Implementation Plan

**Goal:** [one sentence]
**Architecture:** [2-4 sentences describing the final owner-clean shape]

**Intent:** [what must truly change]
**Observable Delta:** [what must be measurably different in the final state]
**Primary Contract:** [owner boundary or public interface that must hold]
**First Proof:** [failing test, repro, validator, benchmark, or artifact to run first]
**First Slice:** [smallest behavior or invariant to prove first]
**Blast Radius:** [which seams/files move together and why they belong in one slice]
**Execution Posture:** [why this should land as one-shot or, if split, the durable reason it cannot]
**Allowed Mechanism:** [valid implementation shapes]
**Forbidden Shortcuts:** [what would look done but is not]
**Forbidden Scaffolding:** [bridges, shims, compile-only staging, dual paths, placeholder adapters, etc.]
**Proof Obligations:** [evidence required before the slice can be accepted]

**Proof Ownership:**
- Architect-owned proof: [boundary, contract, or owner-cut proof]
- Executor-owned proof: [local verification inside the slice]
- Hostile-review focus: [what must be attacked before approval]
- Escalation trigger: [when executor must stop instead of weakening the design]

**Not Done Until:** [plain-language completion bar]

**Solution Integrity Check:**
- Least-painful patch: [...]
- Why rejected: [...]
- Largest owner-clean slice: [...]
- Why this slice is safe for a strong agent: [...]
- If forced to stage: [bounded reason, not convenience]
- Debt or drift path: [...]
```

For `NOVA`, add `Governance Direction`, `Optimization Pressure Test`, and `Solution Integrity Check` before implementation begins whenever the slice is non-trivial.

If the repo doctrine does not name the mechanism directly, say so explicitly and record the bounded inference path.

## Execution Units

Plans should define execution units, not ritual task crumbs.
An execution unit is the biggest owner-clean slice that can land without leaving transitional architecture behind.

Good execution unit size:

- one owner-complete change across all files needed for the final shape
- one contract family or behavior slice with a clear acceptance bar
- one proof surface that validates the final state, not a temporary intermediate
- one reviewable outcome that can merge without cleanup debt

Bad execution unit size:

- "add types first, wire callers later"
- "make it compile, then do the real change"
- "introduce shim/adapter now, remove it in a later task"
- "RED by one agent, GREEN by another"
- "split by file count even though the owner boundary is shared"
- "phase the rollout only because the workload feels large"

Large workload alone is not a reason to split the plan.
If the slice is architecturally coherent, prefer one strong executor over a ladder of transitional subtasks.

## What Each Execution Unit Needs

- target outcome in final-state terms
- owned write scope
- boundary contract
- implementation shape
- proof to create or reuse
- verification command or artifact
- explicit stop conditions
- explicit banned shortcuts for that unit

If tests are the chosen proof surface, describe the first one to three vertical proof slices.
Do not describe a horizontal "write all tests, then all implementation" phase.
Never split `RED` from `GREEN` into separate execution units.

If a unit cannot be described without referencing temporary compatibility scaffolding, the unit is shaped incorrectly.

## Proof Selection

Plans should tell the executor which evidence fits the slice:

- `behavior bug or new behavior`: failing repro or boundary-facing test first when it sharpens the contract
- `refactor`: invariant proof, characterization coverage, or existing strong coverage
- `owner cut or seam change`: boundary inspection plus targeted proof
- `lifecycle or concurrency`: production-path scenario with deterministic control seams and controlled fault injection
- `hard replacement`: owner law and production boundary first, focused local `RED` -> `GREEN`, integrated lifecycle proof, then positive production-path cutover plus build-graph and deletion evidence
- `hot path or perf`: benchmark, repo proof artifact, or perf scenario tied to the final shape
- `docs or mechanical`: targeted validation only

Proof should validate the final architecture.
Choose `RED` -> `GREEN` only after the owner and claimed law are decided, the expected result has explicit authority, the failure is controllable, and the cycle fits one owner-clean execution unit.
A compile failure may prove a decided static contract; it cannot complete a runtime behavior checkpoint.
Do not ask for tests that merely pin a temporary shim or bridge in place.
If a bridge is reluctantly allowed, the proof plan must emphasize bridge removal or explicitly record the surviving debt.

## Shortcut Pressure Test

Before finalizing the plan, answer:

- What is the weakest implementation that could pass while missing the real intent?
- What is the least-painful patch that could technically work?
- What fake-safe staging sequence would leave compile bridges or shims behind?
- Why is that route rejected?
- What is the largest owner-clean slice a strong agent could land instead?

If the plan cannot kill the obvious shortcut, tighten `Forbidden Shortcuts`, `Forbidden Scaffolding`, or `Proof Obligations`.

## Adversarial Validation Gate

Before approving the plan, run this hostile check:

- Does every split correspond to a durable boundary rather than fear of a large diff?
- Could a strong executor land two or more adjacent units as one owner-clean slice instead?
- Does any unit exist only to preserve temporary compatibility or compilation?
- Would the final review still find a bridge, shim, adapter, or dual path that survives only because the plan permitted it?
- Can the executor complete the slice without inventing missing contracts?
- Does any proposed test invent an API, public constructor, owner state, or test-only production hook?
- Does any leaf or compile-only proof claim to complete an owner, lifecycle, behavior, or cutover checkpoint?

If the answer exposes transitional architecture, rewrite the plan before execution starts.

## Delegation Handoff

If the plan will be executed by other agents, produce short copy-ready prompts for:

- `primary executor`
- `hostile reviewer`

Only add extra executor prompts when write scopes are truly disjoint and the split is durable.
Never delegate by separating setup from finish, RED from GREEN, or bridge creation from bridge cleanup.

## Bottom Line

Good plans are built for capable agents.
They assume large one-shot delivery is available, and they only split when the architecture itself demands it.
They remove ambiguity about end state, owner boundary, contract, proof, and stop conditions.
They do not normalize compile bridges, shims, convenience staging, or proofless execution.
