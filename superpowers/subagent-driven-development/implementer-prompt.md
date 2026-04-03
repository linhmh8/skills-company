# Implementer Prompt Template

Use this template when assigning one execution task to a specialized subagent.

Spawn contract:

- model: `gpt-5.4`
- reasoning effort: `high`
- do not use a weaker model for implementation work

## Prompt

```text
Implement this assigned execution task.

Task: [TASK NAME]
Workdir: [DIRECTORY]

Objective:
[FULL OBJECTIVE OR PLAN SUMMARY]

Objective Card:
[CANONICAL NOUNS, OWNER MAP, INVARIANTS, DEBT DOC PATH, VERIFICATION BAR]

Assigned task:
[FULL TASK TEXT]

Required context:
[ONLY THE CONTEXT NEEDED FOR THIS SLICE]

Before coding, state:
- the intent of this slice
- the exact write scope you own
- the proof surface you own locally
- what is explicitly out of scope
- the canonical nouns you will preserve
- the least-painful patch you are rejecting

Rules:
- implement only within your assigned scope
- preserve the canonical nouns and owner boundaries from the Objective Card
- do not rewrite architect-owned proof or boundary contracts
- if a RED test is the right proof, keep RED/GREEN local to your slice
- if another proof surface is stronger, use that instead
- remove any temporary bridge code needed only during your local transition before returning; if you cannot, report it explicitly with the debt-registration note needed
- you were spawned with model `gpt-5.4` and reasoning effort `high`; do not downgrade the task into a weaker-worker shortcut
- stop and report a blocker if the task expands or the plan appears wrong

Return:
- status: DONE | DONE_WITH_CONCERNS | NEEDS_CONTEXT | BLOCKED
- what you changed
- what you verified
- files changed
- temporary bridge code removed: yes | no
- if no, exact bridge code left behind and why
- debt registration required: yes | no
- any concerns or blockers
```
