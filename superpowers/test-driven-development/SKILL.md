---
name: test-driven-development
description: Use when implementing or changing behavior where pre-change evidence should lock the expected outcome before code is written.
---

# Test-Driven Development

## Overview

For Codex, TDD is local evidence inside an evidence-first workflow, not architecture authority.
Use it when a failing test is the sharpest way to lock a contract before implementation drift starts.
When a repro, validator, benchmark, or boundary artifact is stronger, use that instead.

## Core Rules

```text
NO BEHAVIOR CHANGE WITHOUT PRIOR EVIDENCE
```

Usually that evidence is a failing test.
Sometimes the right evidence is a failing repro, a characterization test, a benchmark, or a boundary proof.

```text
LOCK THE CONTRACT BEFORE RED
```

Before writing a test, know what boundary or public interface you are proving and what observable behavior should fail first.

```text
PREFER PUBLIC-INTERFACE PROOF OVER IMPLEMENTATION-SHAPED TESTS
```

## Lock The Contract Before RED

Before the first failing test, pin these four things:

- the owner boundary or public interface being exercised
- the first behavior or invariant to prove
- the observable failure signal
- what is intentionally out of scope for this cycle

If you cannot name these quickly, you probably need `brainstorming` or `writing-plans` before you need TDD.

## Applicability Gate

Use `RED` -> `GREEN` only when all of these are true for the current slice:

- the owner and claimed law are settled enough that the test will not invent architecture or a public API
- the expected result has explicit authority from a spec, accepted decision, invariant, or external oracle
- the failure is deterministic or controlled through a production-valid seam
- `RED` and `GREEN` can land in the same owner-clean execution unit without a compatibility route, temporary owner, test-only state injector, or surrogate path

If any condition is missing, stop TDD and return to design or proof-surface selection.
A leaf test may prove a deterministic mechanism, but it cannot alone close an owner, lifecycle, or cutover claim.

## Use TDD Most Aggressively For

- bug fixes with a clear repro
- new deterministic behavior at an owner boundary
- fail-closed validation, admission, routing, and state-machine law
- parser, serializer, protocol, wire, and schema changes
- state machines and invariants
- regressions that must never return

## Use A Different Proof Surface For

- refactors with no intended behavior change
- thin shims, adapters, and wrappers that mostly pass data through
- intermediate bridge code added only to keep a multi-task refactor or feature plan compiling between old and new shapes
- owner-cut cleanup where semantics stay at the owner boundary
- hot-path or performance work
- docs, config, and mechanical edits

## Do Not Fire For

- helper extraction with unchanged behavior and already-strong coverage
- source cleanup that deletes an old helper, shim, or placeholder
- temporary compile-keeping bridge code that exists only so Task N can land before Task N+1 removes or absorbs it
- exact container choice, inline capacity, scratch layout, pointer identity, or internal staging order unless that shape is itself a recorded contract
- wording, naming, phase-label, or compatibility-alias cleanup
- tests whose only job is to grep source text or prove a symbol disappeared
- duplicate boundary rejection tests already locked at a stronger adjacent owner seam
- one-line dispatch or helper-math tests that restate the implementation without locking a real boundary

If the best proof you can imagine is "assert this helper name no longer exists", you probably chose the wrong proof surface.

## When Test Is The Right Proof Surface

Run tests as vertical proof slices:

1. Choose one behavior at the public interface or owner boundary.
2. `RED`: write one failing test or minimal failing repro for that behavior.
3. `GREEN`: make the smallest owner-clean change that makes that proof pass.
4. Re-run the proof and the adjacent checks that actually matter.
5. Refactor only after the proof is green.

Rules:

- one slice at a time
- do not batch all RED tests first
- do not prebuild future abstractions "for the next test"
- if the current slice reveals the contract is wrong, stop and re-lock the contract before writing more code

## Public-Interface Heuristic

Good tests prove caller-visible behavior:

- inputs go through a real public interface
- assertions target returned values, externally visible state, or boundary-visible effects
- the test would survive a harmless internal refactor

Bad tests prove implementation shape:

- mocking or spying on internal collaborators you own
- asserting helper call order, helper names, or private fields
- verifying behavior by bypassing the interface and inspecting internal storage directly

If a harmless rename or helper extraction would break the test while behavior stays correct, the test is probably wrong.

## Boundary-Only Doubles

Prefer real owned modules on real code paths.
Use doubles only at true boundaries such as:

- third-party services
- time or randomness
- network hops
- storage or filesystem seams when a real local substitute is not practical

Do not mock your own module graph just to make a test easier to write.
If you feel forced to do that, the interface or owner boundary probably needs work.
Production-valid control seams such as clocks, schedulers, randomness sources, and fault injectors are allowed.
Do not add a public hook that exists only to bypass production ownership from tests.

## Pick The Right Evidence

- `Bug or regression`: write a failing test or minimal repro first.
- `New behavior`: write a failing test that shows the intended contract.
- `Protocol or wire change`: write a failing round-trip, encode/decode, layout, or boundary test against real records or bytes. Do not stop at enum arithmetic or source-shape checks.
- `Artifact or validator contract`: assert stable schema keys, manifest fields, phase metadata, and validator behavior. Avoid locking prose wording when semantics stay unchanged.
- `Refactor`: pin current behavior with characterization or invariant proof if existing coverage is weak. If the behavior is already well-covered, do not invent extra tests just because files changed.
- `Thin shim or adapter`: prove at the owner boundary unless the shim performs semantic translation, filtering, retry, caching, fallback, or will live long enough to deserve its own contract.
- `Temporary bridge in a staged plan`: keep proof on the long-lived owner contract before and after the bridge. Do not add unit tests whose only purpose is to stabilize the temporary bridge itself.
- `Owner cut or transition cleanup`: use boundary proof, integration proof, or validator evidence. Do not spray tests across temporary layers scheduled to disappear.
- `Hot path or perf slice`: establish a benchmark, validator, or proof artifact first, then guard correctness separately. Do not use unit tests to lock scratch shape or container details as a proxy for performance.
- `Docs or mechanical change`: use the smallest verification that proves the artifact is valid.

## Relevant Test Gate

Before writing or keeping a test, answer all of these:

1. What owner-boundary contract or invariant does this lock?
2. Would a harmless rename, container swap, or internal refactor break it while behavior stays correct?
3. Is a stronger adjacent owner seam already proving this more directly?
4. Does the test name claim more than the assertion actually proves?
5. If this test disappeared, what real regression would escape?
6. Would reintroducing the target defect or violating the claimed law make it fail?
7. Would a different correct implementation still pass?

If the answers point at helper names, scratch capacity, pointer identity, source text, wording, or duplicate seam coverage, choose a different proof surface.

## Thin-Layer Rule

Do not spray tests across wrappers just because the diff touched wrappers.

One strong boundary test is better than five pass-through tests when:

- the wrapper only forwards data
- the real semantics live in the owner seam
- the wrapper is transitional and should disappear

Write direct wrapper tests only when the wrapper has independent behavior.

## Staged-Plan Bridge Rule

In a multi-task refactor or feature plan, intermediate bridge code may exist only so old and new shapes can coexist without breaking the build.

Treat that bridge as disposable scaffolding unless it has real independent semantics.

Default rule:

- do not write dedicated tests for compile-keeping bridge code
- keep or strengthen proof on the long-lived owner behavior
- let the temporary bridge inherit that proof indirectly

Write direct tests for the bridge only if it independently performs:

- semantic translation
- filtering or routing decisions
- fallback or compatibility policy
- retry, caching, buffering, or ordering law
- a contract expected to survive beyond the staged refactor

## Architecture And Hard Replacements

For owner, lifecycle, cancellation, synchronization, reconnect, teardown, or hard-cut work:

1. lock the owner law, observable contract, and proof obligations
2. create the real production owner boundary
3. apply focused `RED` -> `GREEN` only to deterministic mechanisms inside that owner
4. prove integrated lifecycle with production-path scenarios and controlled fault injection
5. prove cutover with positive production-path behavior plus build-graph and deletion evidence

Do not create a RED-only execution unit, speculative API suite, compatibility route, internal-owner mock, or test-only owner-state injector.

## Compile-Time RED

A compile failure proves a static contract only.
Treat it as valid `RED` when the canonical contract is already decided, the missing capability is narrow, and `GREEN` lands in the same execution unit.
Do not use compile-only `RED` to complete a runtime behavior, lifecycle, owner-boundary, or cutover checkpoint.

## Forbidden Proof Shortcuts

Do not add tests that primarily:

- grep source files with `include_str!` or file reads
- lock helper names, private struct fields, or internal function ordering
- lock exact container types, inline counts, pointer reuse, scratch capacities, or staging traces unless the governing plan or doctrine records that shape as a contract
- prove a shim or helper was deleted by asserting a string disappeared
- lock wording, headings, phase names, compatibility aliases, or doc prose
- compensate for weak boundary reasoning by adding pass-through unit tests
- lock temporary bridge code just because it exists in an intermediate task of a larger plan
- duplicate the same fail-closed rejection at multiple seams when one owner-boundary proof already covers it better

If a test would fail after a harmless rename while behavior stays correct, it is probably the wrong test.

## Refactor Rule

For refactors, the goal is usually:

- preserve behavior
- improve ownership
- reduce complexity

That means the proof should target preserved behavior and seam safety, not every helper that moved.

Good options:

- characterization tests
- invariant checks
- existing integration coverage
- targeted smoke tests for the touched surface
- owner-boundary proof that survives the temporary bridge and still matters after it is removed

Bad options:

- adding a new unit test for every extracted helper and shim
- adding source-guard tests for transition-layer implementation details
- adding tests for temporary bridge code whose only job is to keep the repo compiling between staged tasks

## Codex Guidance

- keep proof close to observable behavior, not implementation details
- when tests are the chosen proof surface, run them as vertical slices instead of a horizontal all-tests phase
- prefer contract-first TDD over implementation-first TDD
- do not add tests only because a process document sounds absolute
- if you already wrote code before RED, do not panic and delete blindly; decide whether the missing piece is proof, scope, or design clarity, then recover honestly

## Smell Checks

Stop and rethink if you are:

- testing mocks instead of behavior
- adding tests for wrappers that only forward arguments
- proving new counters or names instead of the real delta
- proving source emptiness, wording, helper math, or exact stage traces instead of boundary behavior
- pinning exact scratch or container shape when the real risk is alloc/copy/boundedness and a better artifact or invariant exists
- writing more test code than product code for a tiny wrapper or temporary compile-keeping bridge
- adding a unit test to compensate for not understanding the owner boundary
- relying on scheduling hope instead of a deterministic barrier, scheduler, or controlled fault
- adding a constructor, counter, helper, sidecar, or `*_for_test` path only to make `RED` possible
- using a self-reported counter as proof of allocation, copying, joining, fairness, or progress

## Bottom Line

Use failing tests by default when tests are the sharpest proof.
Use a different proof surface when it is clearly stronger.
The goal is not "test first" as a sequencing ritual.
The goal is "evidence first" so implementation stays honest and future refactors stay owner-clean.
