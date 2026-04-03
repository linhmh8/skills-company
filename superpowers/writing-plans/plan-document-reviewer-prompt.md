# Plan Document Reviewer Prompt Template

Use this template when asking a reviewer to pressure-test a written plan.

## Purpose

Verify the plan is complete, aligned with the governing spec, and decomposed clearly enough for execution.

## Prompt

```text
Review this implementation plan for execution readiness.

Plan path: [PLAN_FILE_PATH]
Spec path: [SPEC_FILE_PATH]

Check:
- completeness: missing tasks, placeholders, or hand-wavy steps
- spec alignment: whether the plan covers the governing requirements without scope drift
- task design: whether slices are bounded, reviewable, and executable
- hardening: whether Intent, Observable Delta, Forbidden Shortcuts, Proof Obligations, and Proof Ownership are strong enough to block fake-pass work
- solution integrity: whether the least-painful patch was identified and explicitly rejected
- buildability: whether a strong executor could run this plan without inventing contracts

Calibration:
- flag only issues that would cause real execution trouble
- keep wording nits advisory

Output:
- Status: Approved | Issues Found
- Findings: concrete issues with task or section references
- Advisory suggestions: optional improvements that should not block execution
```
