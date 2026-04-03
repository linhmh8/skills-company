---
name: verification-before-completion
description: Use when about to claim a task is fixed, complete, or passing and you need fresh evidence that matches the exact scope of that claim.
---

# Verification Before Completion

## Overview

Verification is claim-matching.
Run the smallest fresh proof that honestly supports what you are about to say.

## Core Rule

```text
NO CLAIM BROADER THAN FRESH EVIDENCE
```

If you only ran a narrow check, make a narrow claim.

## Workflow

1. State the exact claim.
2. Name the owner boundary or primary contract behind that claim.
3. Choose the freshest command or artifact that proves that exact scope.
4. Run it fresh.
5. Read the real output, not the hoped-for output.
6. Report the result with the right scope.

For multi-task plans, add one more step before the claim:

7. Inspect the touched surface for residual intermediate compile-keeping bridge code and remove it, or narrow the claim to "task batch not fully closed."

## Lock The Claim Before Verifying

Before you run the final proof, pin these explicitly:

- the exact claim you want to make
- the owner boundary or public interface that claim depends on
- the proof artifact that best matches that claim
- what the proof does not cover

If you cannot state these cleanly, the completion claim is not ready.

## Examples

- `crate builds`: run the crate build, not just lint
- `bug fixed`: run the original repro or regression proof
- `touched slice is green`: run the targeted tests for that slice
- `workspace is green`: run the workspace command
- `performance improved`: use the benchmark or proof artifact, not intuition

## Rules

- do not trust old output
- do not trust agent success reports without checking the result yourself
- do not treat a narrow runner as proof for a broader surface
- do not say "should pass" when you can still run the command
- do not treat "latest task is green" as proof that the whole plan is cleanly closed
- do not accept temporary compile-keeping bridge code as a finished end state unless the governing plan or repo log explicitly says it remains on purpose

## Claim-Matching Guidance

- `behavior claim`: verify with the original failing test, repro, or boundary artifact
- `contract claim`: verify at the owner boundary or public interface, not through internal helpers
- `refactor claim`: verify preserved behavior or invariant, not merely compilation
- `performance claim`: verify with benchmark or repo proof artifact, not intuition
- `completion claim`: verify both the proof surface and the absence of residual transition scaffolding

If the best proof you have only shows a smaller truth than the sentence you want to say, change the sentence.

## Codex Guidance

- prefer one exact proof artifact per claim instead of bundling unrelated commands and hand-waving the result
- if the proof artifact changed during implementation, verify the final claim against the final contract, not the old one
- when a task was guided by `writing-plans`, reread the plan header before claiming done
- when a task used `systematic-debugging` or `test-driven-development`, prefer the same locked failing artifact or its direct green successor for closeout

## NOVA Note

Inside `NOVA`, use repo validators when they exist:

- `cargo xtask check` for build and test shape
- `cargo xtask proof-check ...` for proof surfaces that define validators
- prefer `cargo xtask finalize ...` before claiming a non-trivial slice is closed
- if `finalize` is not used, `cargo xtask closeout ...` runs only after the required gate and proof steps

Build proof is not hot-path proof.
Shape proof is not performance proof.

For staged refactors or multi-task plan closure inside `NOVA`:

- reread the governing plan before claiming done
- inspect touched files for bridge code that existed only to keep intermediate old/new shapes compiling
- if any such bridge survives, remove it or report that the plan is not fully complete
- do not let `cargo xtask check` substitute for this cleanup audit

## Bottom Line

Verify the exact claim against the right contract.
Then make only that claim.
