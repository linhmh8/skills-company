---
name: executing-plans
description: Use when you already have a written implementation plan and need to execute it carefully in a separate session or without same-session delegation.
---

# Executing Plans

## Overview

Plans are not scripts.
Read them critically, keep the intent locked, and choose the right proof surface for each slice.

## Workflow

1. Read the governing plan.
2. Lock `Intent`, `Observable Delta`, `Forbidden Shortcuts`, and `Proof Ownership`.
3. Raise gaps before touching code.
4. Execute one bounded slice at a time.
5. Verify with the proof surface the plan actually requires.
6. Stop if implementation pressure tries to rewrite the proof contract.

## Executor Responsibilities

- preserve plan intent
- reject ad hoc patches the plan already ruled out
- keep work inside the assigned scope
- report blockers instead of improvising around them

## Proof Discipline

Every task needs evidence, but not every task needs a new failing unit test.
TDD is local evidence, not the default implementation order.
Use `RED` -> `GREEN` only after the owner, claimed law, expected-result authority, and controllable failure signal are settled; keep both in the same owner-clean execution unit.

Use:

- RED tests or repros for behavior changes
- characterization or invariant proof for refactors
- boundary proof for owner cuts
- production-path scenarios and controlled fault injection for lifecycle or concurrency claims
- positive production-path behavior plus build-graph and deletion evidence for hard replacements
- benchmark or validator artifacts for performance claims
- targeted validation for docs or mechanical changes

A leaf test may prove a mechanism but cannot alone close an owner, lifecycle, or cutover claim.
A compile failure proves a decided static contract only, not runtime behavior.

## Stop Conditions

Stop and escalate when:

- the plan is missing boundary or proof clarity
- the only way forward is to weaken architect-owned proof
- verification keeps failing at the same seam
- the implementation is drifting toward the least-painful patch
- TDD would require inventing an API, compatibility path, temporary owner, internal-owner mock, or test-only production hook

## Closeout

After execution, run the repo-required verification and closeout flow.
If the repo has a dedicated finishing or closeout skill, use it.

## Bottom Line

Execute the plan faithfully, but not blindly.
The executor owns honest implementation and honest verification.
