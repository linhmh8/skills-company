# Trigger And Constraint Design

Use this reference when a skill needs stronger decision guidance but you want to avoid coercive, compliance-heavy wording.

## Goal

Make the right action easy to choose by clarifying:

- when the skill applies
- what risk it is preventing
- what evidence counts
- when the skill should stay out

## Prefer These Levers First

### 1. Clear trigger

State the conditions that make the skill relevant.

Good:
- "Use when a behavior change needs pre-change evidence."
- "Use when a request still has material design uncertainty."

Bad:
- "Use for all tasks."

### 2. Explicit non-goals

Say where the skill should not fire.

Good:
- "Skip for tiny mechanical edits."
- "Do not demand new wrapper tests when owner-boundary proof is stronger."

### 3. Risk-based rule

Tie discipline to a concrete failure mode.

Good:
- "Use a failing repro first for bug fixes so the fix cannot drift."
- "Use benchmark artifacts for hot-path claims; unit tests do not prove performance."

### 4. Claim-matching proof

Tell the agent what evidence fits the claim.

Good:
- `bug fix -> repro or regression proof`
- `refactor -> characterization or invariant proof`
- `owner cut -> boundary proof`

### 5. Stop conditions

Say when the agent must pause rather than improvise.

Good:
- "Stop if the only path forward is weakening architect-owned proof."
- "Stop if the patch needs a flag only to paper over a missing contract."

## Use Strong Language Sparingly

Hard language is justified when:

- safety or correctness risk is high
- the alternative proof is weak
- the agent is likely to rationalize a known bad shortcut

Even then, target the specific risk.
Do not use absolute wording as a substitute for precise triggers.

## Anti-Patterns

- "always" when the correct answer is conditional
- mandatory workflows with no negative trigger
- tool-specific instructions copied from another environment
- rules that optimize obedience instead of good engineering judgment

## Bottom Line

The best skill is not the loudest.
It is the one that changes the decision at the right moment for the right reason.
