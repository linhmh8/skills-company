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

Use:

- RED tests or repros for behavior changes
- characterization or invariant proof for refactors
- boundary proof for owner cuts
- benchmark or validator artifacts for performance claims
- targeted validation for docs or mechanical changes

## Stop Conditions

Stop and escalate when:

- the plan is missing boundary or proof clarity
- the only way forward is to weaken architect-owned proof
- verification keeps failing at the same seam
- the implementation is drifting toward the least-painful patch

## Closeout

After execution, run the repo-required verification and closeout flow.
If the repo has a dedicated finishing or closeout skill, use it.

## Bottom Line

Execute the plan faithfully, but not blindly.
The executor owns honest implementation and honest verification.
