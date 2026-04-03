---
name: requesting-code-review
description: Use when a risky or non-trivial change needs an independent review for bugs, regressions, proof gaps, or boundary drift before merge or closeout.
---

# Requesting Code Review

## Overview

Use review as risk reduction, not theater.
Ask for review when fresh eyes can realistically catch correctness, ownership, or proof problems.

## Request Review When

- finishing a non-trivial slice
- before merge or major handoff
- after a boundary, protocol, or hot-path change
- when plan conformance is uncertain
- when you want an adversarial pass on a risky bug fix

## Skip Mandatory Review When

- the change is tiny and mechanical
- the risk is already low and verification is strong
- review would just restate obvious facts without finding new failure modes

## Give The Reviewer A Real Scope

Provide:

- the diff, commit range, or touched paths
- the governing plan or spec when one exists
- the primary contract or owner boundary that changed
- the claimed verification evidence
- any known risk areas

Good review questions:

- what can break
- what proof is missing
- where is owner drift hiding
- what shortcut could still fake-pass

## Reviewer Focus

Prioritize:

- bugs and regressions
- proof gaps
- boundary and owner drift
- hot-path costs
- tests that only prove the implementation
- low-value test bloat around shims or wrappers

In `NOVA`, use `nova-review` when repo doctrine and seam ownership are central to the risk.

## Before You Ask For Review

Pin these explicitly:

- what claim you think the change now supports
- what proof artifact actually supports that claim
- what part of the touched surface still feels risky
- what reviewer should try hardest to falsify

Weak review requests produce weak reviews.

## Handling Findings

- fix high-severity findings before moving on
- push back on wrong findings with code and evidence
- do not cargo-cult every suggestion
- if the fix path itself is ad hoc, redesign instead of papering over the finding

## Codex Guidance

When subagents are available and the user wants delegation, use one hostile reviewer with a bounded scope.
If delegation is not appropriate, run the same checklist yourself in review mode.
Do not ask for review with only a vague "see if this looks good"; name the contract and the proof claim.

## Bottom Line

Review should increase signal.
If it does not change decisions, scope it better or skip it.
