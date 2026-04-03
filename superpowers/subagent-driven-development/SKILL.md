---
name: subagent-driven-development
description: Use only when the user explicitly asks for subagents, delegation, or parallel agent work and wants coordinator-managed task decomposition, per-task execution by GPT-5.4 high subagents, and a final GPT-5.4 xhigh audit.
---

# Subagent-Driven Development

## Overview

Use subagents only when delegation is explicitly requested.
Run the work as a coordinator-managed task graph: lock the objective vocabulary first, spawn one specialized implementation subagent per execution task, verify each result against the objective, then run a separate senior audit over the integrated whole.

## Use When

- the user explicitly asks for subagents, delegation, or parallel agent work
- the objective can be decomposed into concrete execution tasks
- the coordinator can define a canonical vocabulary and owner map before spawning
- write scopes can stay disjoint or integration order can be enforced cleanly
- the user wants stronger workflow verification, not just faster execution

## Do Not Use When

- the user did not explicitly ask for delegation
- the same unresolved architectural decision would be handed to multiple agents
- there is no clean task decomposition after the vocabulary is locked
- the work is small enough that delegation and audit overhead dominates
- the coordinator cannot name where temporary bridge code or debt registration belongs

## Hard Gate

- Do not spawn subagents unless the user explicitly asked for delegation or parallel agent work.
- Before spawning anyone, produce an `Objective Card` locally with:
  - canonical nouns for the feature or refactor
  - owner boundaries and write-scope map
  - required invariants and verification bar
  - integration order when tasks are not fully parallel
  - the debt register path for any temporary bridge code that cannot be removed immediately
- Resolve blocking architecture decisions locally before delegation.
- Spawn every implementation subagent with model `gpt-5.4` and reasoning effort `high`.
- Run the final senior audit with model `gpt-5.4` and reasoning effort `xhigh`.
- Do not use weaker implementation workers or skip the final audit.

## Core Principle

Split by execution task only after the semantic contract is locked.
Do not ask multiple agents to invent vocabulary, choose owners, or negotiate boundaries independently.

## Roles

- `Coordinator`: owns objective locking, task decomposition, delegation, per-task verification, integration, and bridge-code disposition.
- `Sub-Agent`: owns one execution task with explicit scope, proof expectations, and return contract.
- `Senior Auditor`: performs a final whole-system review after integration using stronger reasoning and no authorship of implementation tasks.

## Workflow

1. Read the objective, governing plan, and repo doctrine locally.
2. Write the `Objective Card` before any spawn. Lock vocabulary, owner boundaries, constraints, verification targets, and the debt register location.
3. Initialize the acceptance ledger from [task-ledger-template.md](task-ledger-template.md) and keep it as the coordinator's live source of truth.
4. Break the objective into execution tasks. Merge trivial tasks before spawning so the final task list is meaningful and stable.
5. Register each task in the ledger before spawning it.
6. Spawn one specialized implementation `Sub-Agent` per execution task with model `gpt-5.4` and reasoning effort `high`.
7. Pass each subagent only:
   - its task statement
   - the relevant subset of the `Objective Card`
   - owned files or write scope
   - required proof
   - stop conditions
8. Require each subagent to report exact changes, proof run, unresolved risks, and any temporary bridge code introduced.
9. Verify each returned task result individually against the objective and project requirements before marking it accepted.
10. Record the acceptance decision, proof result, and bridge-code disposition in the ledger immediately after review.
11. Integrate the accepted task outputs centrally. Normalize naming and boundary language during integration; do not preserve inconsistent nouns just because a subagent used them.
12. Record terminology normalization, boundary correction, and integration-only bridge cleanup in the ledger's integration section.
13. Remove any temporary bridge code that only existed to keep intermediate states compiling. If something must remain, register it in the debt document named in the `Objective Card`.
14. Run the pre-audit gate from the ledger before spawning the final audit.
15. After all task results are integrated and verified, spawn a separate `Senior Auditor` with model `gpt-5.4` and reasoning effort `xhigh` for a comprehensive final review.
16. Resolve or explicitly record every material auditor finding before closing the work.

## Coordinator Rules

- own the nouns, invariants, and owner map centrally
- do not let subagents rename the same semantic differently without a deliberate coordinator decision
- verify every task individually before integration
- keep a running acceptance ledger with task status, proof status, integration status, and bridge-code status
- if a task expands beyond its assigned boundary, stop and re-slice rather than letting a worker absorb adjacent scope
- if there is no clean task split after vocabulary lock, stop delegating and continue locally

## Prompt Contract

Every delegated prompt should state:

- exact task goal
- canonical nouns and invariants from the `Objective Card`
- owned files or scope anchor
- files the agent must not edit when relevant
- model `gpt-5.4` and reasoning effort `high` for implementation subagents
- proof to run or proof it must not weaken
- bridge-code rule: remove temporary transition code before return, or report it explicitly for debt registration
- stop conditions and when to return blocked
- that the agent is not alone in the codebase and must not revert others' edits

For implementation subagents, require a short return summary with:

- scope handled
- files changed
- proof run or not run
- temporary bridge code removed or retained
- debt registration needed or not needed
- blockers
- remaining risks

## Verification Discipline

- Verify each task against the task statement and the `Objective Card`, not just against the subagent's own summary.
- Use the ledger's acceptance check format so every task is judged on the same axes.
- Reject outputs that satisfy the local task but violate shared vocabulary, owner boundaries, or integration constraints.
- Do not let passing local proof substitute for project requirement coverage.
- Do not run the final audit until the ledger pre-audit gate is satisfied.
- Run the `Senior Auditor` only after integration is materially complete; the auditor is a whole-workflow review, not a patch author.

## Red Flags

- spawning subagents before the coordinator locks vocabulary and owner boundaries
- treating every sentence in a plan as a separate task instead of defining stable execution slices
- letting multiple subagents invent nouns for the same semantic
- accepting task output without individual requirement checks
- integrating temporary bridge code and forgetting to remove it or register it as debt
- using the final auditor as an excuse to skip coordinator verification
- spawning implementation subagents with any model weaker than `gpt-5.4` or with reasoning below `high`
- skipping the separate `Senior Auditor` or downgrading it below `xhigh`

## Bottom Line

This skill is for users who explicitly want a coordinator-run, subagent-heavy workflow.
The coordinator owns semantic cohesion; the subagents own execution; the senior auditor owns the final hostile double-check.
