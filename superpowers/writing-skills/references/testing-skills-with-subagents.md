# Testing Skills With Representative Scenarios

Use this reference when you want to verify that a skill changes real behavior instead of only sounding persuasive.

## Goal

Test whether the skill:

- triggers when it should
- stays out when it should not
- changes decisions under realistic pressure
- matches Codex tool and workflow reality

## Core Rule

Do not "test" a skill by rereading it and deciding it sounds good.
Test it against representative scenarios.

## Scenario Types

Every skill should have at least:

- `positive trigger`: the skill should load and help
- `negative trigger`: the skill should stay out
- `pressure case`: the skill should still guide the right choice under time, sunk cost, or ambiguity
- `environment case`: the skill should not assume unavailable tools or the wrong platform

## Good Tests

Ask the agent to act, not just to explain:

- "You have a clear repro and one owner seam. Do you brainstorm first or fix it?"
- "This refactor moved helpers and thin adapters but should not change behavior. What proof do you add?"
- "You already have strong integration coverage. Do you add wrapper unit tests for a pass-through shim?"
- "You need a review on a tiny mechanical rename. Do you request one?"

## What To Measure

- whether the skill triggered
- what evidence surface it chose
- whether it created unnecessary ceremony
- whether it matched the environment
- whether it protected the real risk

## Common Failure Modes

- skill overfires on trivial work
- skill underfires on risky work
- skill demands the wrong proof surface
- skill uses stale tool names or platform assumptions
- skill rewards ritual instead of outcome

## Rewrite Loop

When a skill fails:

1. capture the exact wrong decision
2. identify whether the problem is trigger, workflow, or environment
3. rewrite the smallest section that fixes that failure
4. rerun the same scenario

## Bottom Line

Skills should survive realistic scenarios, especially the ones that tempt a strong model to skip discipline.
