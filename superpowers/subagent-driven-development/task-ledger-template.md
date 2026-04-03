# Task Ledger Template

Use this template as the coordinator's acceptance ledger for the whole subagent-driven workflow.
Create one ledger per objective before spawning implementation subagents.
Keep it updated as the source of truth for task status, proof status, integration status, and bridge-code disposition.

## Header

```text
Objective:
[FULL OBJECTIVE OR PLAN]

Coordinator:
[NAME OR AGENT ID]

Objective Card:
- canonical nouns: [LOCKED VOCABULARY]
- owner map: [OWNER / WRITE-SCOPE SUMMARY]
- invariants: [NON-NEGOTIABLE REQUIREMENTS]
- verification bar: [REQUIRED PROOF SURFACE]
- debt register path: [DOC PATH]
- integration order: [ORDER OR "FULLY PARALLEL"]
```

## Task Table

Use one row per execution task.

```text
| Task ID | Task Name | Owner Sub-Agent | Write Scope | Depends On | Status | Proof Status | Integration Status | Bridge Code Status | Debt Registration | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| T1 | [NAME] | [AGENT ID] | [FILES / SCOPE] | [NONE / TASK IDS] | Planned | Not Run | Not Integrated | None | Not Needed | [SHORT NOTE] |
```

## Status Vocabulary

Use only these values so the ledger stays comparable across tasks:

- `Status`: `Planned` | `In Progress` | `Returned` | `Accepted` | `Rejected` | `Blocked`
- `Proof Status`: `Not Run` | `Passed` | `Failed` | `Insufficient` | `Not Applicable`
- `Integration Status`: `Not Integrated` | `Integrated` | `Reworked During Integration`
- `Bridge Code Status`: `None` | `Removed Before Return` | `Removed During Integration` | `Retained With Debt` | `Retained By Mistake`
- `Debt Registration`: `Not Needed` | `Recorded` | `Missing`

## Acceptance Check Per Task

Record this block when a task is reviewed.

```text
### [TASK ID] Acceptance Check
- Assigned requirements:
  [TASK REQUIREMENTS]
- Returned summary:
  [SUB-AGENT RETURN]
- Verification result:
  Approved | Rejected
- Requirement gaps:
  [NONE OR LIST]
- Vocabulary or owner drift:
  [NONE OR LIST]
- Proof assessment:
  [SUFFICIENT / INSUFFICIENT WITH REASON]
- Bridge code assessment:
  [NONE / REMOVED / RETAINED WITH DETAILS]
- Debt registration check:
  [NOT NEEDED / RECORDED / MISSING]
- Coordinator disposition:
  [ACCEPT / REWORK / RESLICE / BLOCKED]
```

## Integration Notes

Use this section to record naming normalization, boundary corrections, and any integration-only fixes.

```text
### Integration Pass [N]
- tasks integrated: [TASK IDS]
- terminology normalized:
  [NONE OR LIST]
- boundary corrections:
  [NONE OR LIST]
- bridge code removed during integration:
  [NONE OR LIST]
- debt entries added:
  [NONE OR LIST WITH DOC PATH]
- regressions introduced during integration:
  [NONE OR LIST]
```

## Pre-Audit Gate

Do not spawn the senior auditor until every item below is true.

```text
- all execution tasks are either Accepted or explicitly Blocked with recorded reason
- every Accepted task has a completed Acceptance Check
- proof status is not unknown for any Accepted task
- bridge code is either removed or recorded in the debt register
- integrated result reflects the final naming and owner boundaries
```
