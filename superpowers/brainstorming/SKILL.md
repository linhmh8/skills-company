---
name: brainstorming
description: Use when a request still has open product, UX, architecture, or seam decisions and coding immediately would be guesswork.
---

# Brainstorming

## Overview

For Codex, design work is a scalpel, not a tax.
Use brainstorming when the shape is still open or the cost of guessing is high.
Do not force a full design ritual onto trivial, local, or mechanical work.

## Use When

- the request has multiple viable approaches
- success criteria are unclear
- the work crosses seams, crates, services, or user-visible flows
- the user asks for options, ideas, or recommendations
- the implementation route is still ambiguous enough that coding immediately would be guesswork

## Skip The Full Workflow When

- the bug already has a clear repro and owner
- the change is mechanical, documentation-only, or a rename
- the codebase already has an obvious local pattern to follow
- the task is so bounded that a short assumption statement is enough

## Workflow

1. Inspect the current context first.
2. Identify the real unknowns and risks.
3. Lock a tentative owner boundary or public interface before discussing solutions in detail.
4. Lock the proof focus: what artifact would fail if the choice is wrong, and what behavior matters first.
5. Ask only the questions that unblock decisions.
6. Present 2-3 approaches only when there is a real choice.
7. Recommend one route and say why.
8. Compare the least-painful patch against the long-lived route.
9. Write a design doc or gate only when the slice is non-trivial, user-facing, or repo policy requires it.

## Lock Before Leaving Brainstorming

Before moving from brainstorming into coding or formal planning, pin:

- the chosen owner boundary or public interface
- the success condition in observable terms
- the first contract or invariant to prove
- the strongest proof surface for that first contract
- what is intentionally out of scope for the first slice

If these are still fuzzy, you are not done brainstorming.

## What Good Brainstorming Produces

- a clear intent
- visible success criteria
- explicit constraints
- the chosen owner boundary
- the proof surface that will show the change is real
- the first behavior or invariant to prove
- the reason the rejected shortcuts stay rejected

## Codex Guidance

- keep the loop fast
- do not ask questions whose answers you can discover locally
- do not block obvious execution behind a mandatory approval dance
- when the decision is material, give your recommendation and a brief reason
- if tests will be the proof surface, identify the first behavior to prove instead of sketching a full speculative test suite

## Bottom Line

Brainstorm when design uncertainty is real.
Do not leave the mode with only vague ideas.
Leave with a boundary, a contract, and a proof focus.
