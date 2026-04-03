# Skill Discovery And Trigger Testing

Use these scenarios to test whether a skill library or router prompt causes good trigger decisions without forcing ceremony.

## Scenario 1: Clear Bugfix

```text
You have a deterministic failing repro in one module.
The owner seam is obvious and the required change is local.

Do you:
A) brainstorm a design first
B) lock the repro as proof and fix the owner

Explain why.
```

Expected:
- skip full brainstorming
- choose evidence-first debugging and local fix flow

## Scenario 2: Refactor With Thin Adapters

```text
You are refactoring a module.
Behavior should not change.
The diff adds two thin pass-through adapters and extracts three helpers.
Existing integration coverage is already strong.

Do you:
A) add unit tests for every helper and adapter
B) rely on characterization or integration proof and only add local tests where semantics changed

Explain why.
```

Expected:
- reject wrapper test bloat
- choose proof that targets preserved behavior

## Scenario 3: Non-Trivial Multi-Seam Change

```text
The request affects protocol, runtime ownership, and user-visible behavior across several files.
There are multiple viable designs and unclear proof ownership.

Do you:
A) start coding from the request
B) create a bounded design or implementation plan first

Explain why.
```

Expected:
- trigger planning or brainstorming
- mention owner boundary and proof ownership

## Scenario 4: Tiny Mechanical Edit

```text
The task is to rename one variable and fix a typo in nearby docs.
No behavior changes.

Do you:
A) invoke TDD, planning, and code review workflows
B) make the edit and run targeted validation only

Explain why.
```

Expected:
- skip heavyweight process
- choose narrow verification

## Scenario 5: Perf Claim

```text
You changed a hot-path loop and believe it is faster.
Tests are green.

Do you:
A) claim the performance improvement
B) require benchmark or proof artifacts before making that claim

Explain why.
```

Expected:
- require benchmark or proof artifacts
- avoid overclaiming from shape-only tests

## Scenario 6: Staged Refactor Bridge Code

```text
You are in Task 1 of a larger refactor plan.
To keep the repo compiling, you introduce temporary bridge code between the old shape and the new shape.
Task 2 or Task 3 will remove or absorb this bridge.
Existing integration coverage already exercises the long-lived owner boundary.

Do you:
A) add unit tests for the temporary bridge so the intermediate task feels "complete"
B) keep or strengthen proof on the long-lived boundary and avoid dedicated tests for the temporary bridge

Explain why.
```

Expected:
- reject temporary-bridge test bloat
- choose owner-boundary or integration proof that still matters after the bridge is gone

## Scenario 7: Wording And Doc Cleanup

```text
The task updates wording, phase labels, and compatibility aliases in code comments, docs, and artifact prose.
No runtime behavior changes.

Do you:
A) add tests that lock the exact wording so the cleanup cannot drift
B) make the edit and run targeted validation only

Explain why.
```

Expected:
- skip TDD and heavyweight process
- reject wording-lock tests
- choose targeted validation only

## Evaluation

A skill or router prompt is working if it:

- triggers the right skill on scenarios 1, 3, and 5
- stays lightweight on scenario 4
- avoids test bloat on scenario 2
- rejects temporary-bridge test bloat on scenario 6
- stays lightweight on scenario 7
- names the right proof surface for each case

It is overfiring if it tries to force the same process on all seven scenarios.
