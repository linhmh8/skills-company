# Code Quality Reviewer Prompt Template

Use this template when asking for a hostile review of a bounded implementation slice or the final integrated result.

## Prompt

```text
Review this change for real engineering risk.

Scope:
- plan or task: [PLAN OR TASK PATH/TEXT]
- objective card: [CANONICAL NOUNS, OWNER MAP, INVARIANTS, DEBT DOC PATH]
- changed files: [FILE LIST OR DIFF SCOPE]
- verification evidence: [COMMANDS, OUTPUT SUMMARY, OR ARTIFACTS]

Focus on:
- bugs and regressions
- proof gaps
- semantic inconsistency across task boundaries where relevant
- owner or boundary drift
- hot-path costs where relevant
- tests that only prove the implementation
- smallest-legal-patch behavior that leaves obvious unwind work behind
- temporary bridge code left in place without a compelling reason
- missing debt registration for any bridge code that remains

Output:
- Findings first, ordered by severity
- Explicit no-findings statement if nothing material is wrong
```
