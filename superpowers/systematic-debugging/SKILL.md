---
name: systematic-debugging
description: Use when encountering a bug, failing test, regression, or unexpected behavior and you need root cause before fixing anything.
---

# Systematic Debugging

## Overview

Guessing creates bug piles.
Debugging starts by bounding the failure, locking the failing contract, and tracing ownership before changing code.

## Core Rule

```text
NO FIX WITHOUT A ROOT-CAUSE EVIDENCE CHAIN
```

If you cannot explain where the bad behavior originates, why it happens, and what proof artifact exposes it, you are not ready to fix it.

## Use For

- test failures
- production bugs
- integration mismatches
- performance regressions
- build or runtime breakage

## Workflow

1. Reproduce the failure consistently.
2. Bound the scope.
3. Lock the failing contract and first proof artifact.
4. Trace the data, state, or contract through the owning seam.
5. Find the first divergence between expected truth and actual truth.
6. Compare against a working case, doctrine, spec, or adjacent implementation.
7. Change one thing at a time.
8. Re-run proof and confirm no new drift.

## Bound The Failure

Before fixing, answer:

- what is failing exactly
- where it first becomes observable
- which seam or component owns that truth
- what changed recently
- whether the failure is deterministic or load/timing-dependent

## Lock The Contract Before The Fix

Before editing code, pin these explicitly:

- the owner boundary or contract that is failing
- the first observable behavior or invariant that proves the failure
- the artifact you will re-run after every attempt
- what is still unknown

If these are fuzzy, keep investigating instead of patching.

## Lock Proof Before The Fix

Pick the smallest artifact that proves the problem:

- `behavior bug`: failing test or minimal repro
- `integration bug`: failing contract or boundary test, or a captured trace diff
- `refactor regression`: characterization test or preserved-output check
- `perf regression`: benchmark or proof artifact that shows the regression
- `timing issue`: deterministic probe or event trace, not arbitrary sleeps

If the bug lives across multiple layers, instrument each boundary once to see where truth diverges.
Prefer one tight failing artifact over a grab bag of partial clues.

## Find The First Divergence

Do not stop at the place where the failure becomes visible.
Trace until you can name the first point where:

- expected contract says one thing
- actual state, data, or output says another
- the owning seam can now be held responsible

The visible symptom is not enough.
The first divergence is what turns debugging into engineering instead of guesswork.

## Owner-Cut Discipline

Fix at the owning seam.

Stop if the proposed fix relies on:

- a new boolean or flag that only papers over a missing contract
- a compatibility shim that keeps wrong ownership alive
- a transition wrapper that peeks into another seam's private truth
- silent fallback that hides malformed or impossible state

If a thin adapter appears in the stack, prove the owner boundary first.
Do not write a pile of adapter tests unless the adapter has its own semantics.

## Hypothesis Loop

For each attempt:

1. State one hypothesis.
2. Name what observation would falsify it.
3. Make the smallest change or probe that tests it.
4. Verify the result against the locked artifact.
5. If wrong, discard that hypothesis and update your model.

If you fail repeatedly at the same seam, stop patching and question the architecture or contract.

## Red Flags

- "This quick fix is probably fine"
- "I know what it is without reproducing it"
- "Let's change three things and see"
- "I'll add tests after it works"
- "The shim can absorb this for now"
- "The benchmark is noisy, so I'll trust intuition"

Any of these means you are drifting back into guess-and-check.

## Codex Guidance

- lock the failing contract before discussing fixes
- keep the proof artifact stable while testing hypotheses
- prefer one boundary-facing failing artifact over many weak internal checks
- if the failure contract changes during investigation, stop and re-lock it before editing code

## NOVA Note

Inside `NOVA`, root cause is not enough by itself.
The fix also has to land in the doctrine-owned seam and use the right proof surface:

- RED tests for deterministic behavior failures
- boundary proof for owner cuts and transition removal
- replay or bench artifacts for hot-path claims

## Bottom Line

Find where truth first diverges.
Prove it with a stable artifact.
Fix the owner.
Verify the exact claim.
