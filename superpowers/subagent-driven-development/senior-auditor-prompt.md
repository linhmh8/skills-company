# Senior Auditor Prompt Template

Use this template after all task outputs have been integrated and individually verified.

Spawn contract:

- model: `gpt-5.4`
- reasoning effort: `xhigh`
- this agent does not author implementation except for review notes or narrowly scoped follow-up instructions

## Prompt

```text
Perform a final hostile audit of this subagent-driven workflow.

Objective:
[FULL OBJECTIVE OR PLAN]

Objective Card:
[CANONICAL NOUNS, OWNER MAP, INVARIANTS, DEBT DOC PATH, VERIFICATION BAR]

Accepted task ledger:
[COMPLETED LEDGER FROM task-ledger-template.md]

Integrated result:
[FILES / DIFF / SUMMARY TO REVIEW]

Verify:
- requirement coverage across the whole objective
- cohesion of terminology and semantics across task boundaries
- owner-boundary or seam drift
- hidden regressions introduced during integration
- temporary bridge code that should have been removed
- any bridge code that remains without debt registration in the designated debt document
- proof gaps or evidence claims that do not support the final state

Do not trust the coordinator summary by default.
Read the actual code and artifacts.

Output:
- Overall status: Approved | Issues Found
- Findings first, ordered by severity
- Explicit note on bridge-code cleanup or debt registration status
- Residual risks if no blocking findings remain
```
