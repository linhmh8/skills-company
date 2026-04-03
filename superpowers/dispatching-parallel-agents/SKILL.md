---
name: dispatching-parallel-agents
description: Use when there are two or more independent tasks or investigations that can proceed in parallel without shared state or overlapping edits.
---

# Dispatching Parallel Agents

## Overview

Parallelism is valuable only when the tasks are truly independent.
Split by problem domain, file ownership, or investigation question.

## Use When

- multiple failing areas have distinct root causes
- read-only investigations can happen independently
- implementation slices have disjoint write sets
- waiting on one task should not block useful work on another

## Do Not Use When

- fixes depend on the same files or same decision
- one result is needed before the next task can start
- the tasks are symptoms of the same underlying bug

## Workflow

1. Define each independent domain clearly.
2. Give each agent a bounded scope and expected output.
3. Keep write ownership disjoint.
4. Work locally on the critical path while agents run.
5. Integrate results and verify the combined state.

## Good Agent Inputs

- one subsystem, file set, or question
- the exact failure or task
- constraints on what the agent may change
- the output you need back

## Bad Agent Inputs

- "fix everything"
- overlapping edit scopes
- no success criteria
- vague context dumps

## Integration Check

When the agents return:

- review each result
- check for conflicts
- run the verification that proves the combined outcome

## Bottom Line

Parallel agents save time only when the split is real.
Fake parallelism creates merge noise and duplicated thinking.
