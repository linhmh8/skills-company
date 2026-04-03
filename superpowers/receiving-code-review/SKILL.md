---
name: receiving-code-review
description: Use when receiving review feedback and you need to evaluate it technically before fixing, accepting, or pushing back.
---

# Receiving Code Review

## Overview

Review feedback is input, not a command.
Evaluate it against the codebase, plan, contract, and proof surface before changing code.

## Workflow

1. Read the feedback fully.
2. Separate clear items from unclear ones.
3. Identify which contract, invariant, or claim each finding is attacking.
4. Verify the finding against code, tests, docs, and context.
5. Decide whether the feedback is correct, incomplete, or wrong for this codebase.
6. For important findings, compare the least-painful patch against the long-lived route.
7. Implement or push back with technical reasoning.

## If Feedback Is Unclear

Do not partially implement a multi-item review when key items are unclear.
Ask for clarification first.

## If Feedback Is Correct

Prefer factual acknowledgment:

- `Fixed in [path]`
- `Verified and fixed`
- `Good catch: [issue]. Fixed in [path]`

Do not add performative praise or gratitude just to sound agreeable.

## If Feedback Seems Wrong

Push back with evidence:

- the current contract
- working behavior
- compatibility constraints
- test or benchmark results
- governing plan or repo doctrine

If you cannot verify the suggestion quickly, say what is missing and what investigation would be needed.

## Evaluate The Finding, Not Just The Patch

Before acting on a finding, ask:

- what claim is the reviewer making
- what contract or invariant would fail if they are right
- what proof artifact supports or refutes that claim
- whether the proposed fix is only the smallest legal patch

If the finding is right but the suggested patch is weak, keep the finding and reject the patch shape.

## External Reviewer Guidance

Before accepting an external suggestion, check:

- is it technically correct for this codebase
- does it break existing behavior
- does it ignore important local context
- is it solving the right problem
- is it just the smallest legal patch

## P1/P2 Rule

For higher-severity findings, do not close the thread with the smallest patch that technically works unless a bounded defer is explicit.
Write down:

- the least-painful patch
- the long-lived route
- why you are choosing one over the other

## YAGNI Filter

If a review asks for "proper" machinery, first check whether the feature or path is actually used.
Do not add infrastructure for a surface that does not matter.

## Codex Guidance

- keep the same proof artifact in play when validating or rejecting a finding
- if the review attacks your verification claim, rerun or replace the proof before arguing
- if the review reveals the original contract was underspecified, update the contract before patching the code

## Bottom Line

Review feedback should sharpen the code, not trigger reflex agreement.
Verify first, then fix or push back.
