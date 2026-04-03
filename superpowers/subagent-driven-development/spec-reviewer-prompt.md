# Spec Compliance Reviewer Prompt Template

Use this template when verifying an individual task result against its assignment and the shared objective contract.

## Prompt

```text
Review whether this implementation matches its assigned task.

Objective:
[FULL OBJECTIVE OR PLAN SUMMARY]

Objective Card:
[CANONICAL NOUNS, OWNER MAP, INVARIANTS, DEBT DOC PATH, VERIFICATION BAR]

Requested task:
[FULL TASK REQUIREMENTS]

Implementer report:
[IMPLEMENTER REPORT]

Inspect the actual changed files and verify:
- missing requirements
- extra scope or overbuild
- misunderstood requirements
- ad hoc shortcuts that satisfy the letter but miss the intended route
- semantic drift from the canonical nouns
- owner drift or proof drift
- temporary bridge code that should have been removed
- missing debt registration when temporary bridge code remains

Do not trust the report by default.
Read the actual code and compare it to the task.

Output:
- Status: Approved | Issues Found
- Findings: concrete issues with file references when possible
- Advisory suggestions: optional improvements
```
