# AI-Assisted Development Workflow

## Project

**Repository:** `cifar10-cnn-classifier`

## Objective

Use a simple three-tool development workflow:

```text
Cursor
  ↓
Implementation
  ↓
Codex
  ↓
Testing + Technical Review
  ↓
Cursor
  ↓
Fixes (if required)
  ↓
Codex
  ↓
Re-test / Re-review
  ↓
Claude
  ↓
Final Validation
```

The purpose of this workflow is to keep responsibilities clear:

- **Cursor** builds the code.
- **Codex** independently tests and reviews what Cursor built.
- **Claude** performs the final validation after implementation and review are complete.

---

# 1. Cursor — Primary Implementation Tool

Cursor is the main development tool.

Its responsibility is to implement the requested task in the repository.

## Cursor should

1. Inspect the existing repository before making changes.
2. Understand the current study/project phase.
3. Reuse existing code where appropriate.
4. Implement only the requested scope.
5. Keep the code simple and readable.
6. Avoid unnecessary abstractions or over-engineering.
7. Add or update tests where the task requires them.
8. Run relevant tests before finishing.
9. Clearly report what was changed.

## Cursor should not

- redesign unrelated parts of the project,
- implement future phases early,
- add unnecessary frameworks,
- silently weaken tests,
- hide errors,
- claim completion if tests are failing.

## Cursor completion response

At the end of each implementation task, Cursor should report:

```text
1. What was implemented
2. Files created or modified
3. Important implementation decisions
4. Tests added or updated
5. Test command used
6. Test result
7. Any limitation or unresolved issue
```

---

# 2. Codex — Independent Tester + Technical Reviewer

Codex does **not** act as the primary implementer.

Its main responsibility is to independently verify Cursor's work.

Codex has two jobs:

```text
Testing
+
Code Review
```

## A. Testing responsibility

Codex should:

1. Inspect the actual implementation.
2. Run the relevant test suite.
3. Verify existing tests still pass.
4. Add focused tests if important behavior is not covered.
5. Test important edge cases.
6. Confirm the implementation behaves as requested.
7. Check that no unrelated functionality was broken.

Codex must not rely only on Cursor's reported test results.

It should verify them independently.

## B. Review responsibility

Codex should review:

- correctness,
- simplicity,
- readability,
- maintainability,
- modularity,
- test coverage,
- edge cases,
- scope compliance,
- unnecessary complexity,
- regressions.

For CNN-related work, Codex should also verify mathematical and tensor-shape correctness where relevant.

Examples:

```text
input channels
output channels
kernel size
padding
stride
tensor dimensions
flattened dimensions
Linear layer input size
number of classes
parameter calculations
```

## Codex review severity

Use:

```text
BLOCKER
MAJOR
MINOR
OPTIONAL
```

### BLOCKER

The implementation cannot be accepted.

Examples:

- code does not run,
- core tests fail,
- wrong CNN dimensions,
- incorrect model architecture,
- data pipeline is broken,
- implementation does not satisfy the requested task.

### MAJOR

Important problem that should be fixed before final validation.

Examples:

- incorrect edge-case handling,
- fragile implementation,
- missing important tests,
- duplicated or misleading logic.

### MINOR

Small non-blocking issue.

Examples:

- naming,
- documentation,
- small cleanup,
- minor readability improvement.

### OPTIONAL

Nice-to-have only.

Do not block progress for optional improvements.

## Codex verdict

Return one:

```text
APPROVED
APPROVED WITH MINOR FIXES
CHANGES REQUIRED
```

If changes are required, Codex should give a finite list of exact fixes.

---

# 3. Cursor — Fix Review Findings

If Codex reports:

```text
CHANGES REQUIRED
```

the task returns to Cursor.

Cursor should receive only the relevant review findings and implement the required corrections.

The cycle becomes:

```text
Cursor implementation
        ↓
Codex review
        ↓
Issues found
        ↓
Cursor fixes
        ↓
Codex re-test
```

Do not send the work to Claude while BLOCKER or MAJOR issues remain unresolved.

---

# 4. Codex — Re-Test After Fixes

After Cursor fixes the findings, Codex should verify:

1. Each required issue was actually fixed.
2. No regression was introduced.
3. Relevant tests pass.
4. The original task still behaves correctly.
5. No new BLOCKER or MAJOR issue exists.

Once satisfied, Codex should return:

```text
APPROVED
```

or, when only genuinely non-blocking issues remain:

```text
APPROVED WITH MINOR FIXES
```

At this point the implementation can move to Claude.

---

# 5. Claude — Final Validator

Claude is the final validation layer.

Claude should **not automatically rewrite the implementation**.

Its responsibility is to inspect the completed work and decide whether the task is truly ready to be considered finished.

Claude should review:

- Cursor's implementation,
- Codex's review findings,
- Cursor's fixes,
- final test results,
- repository state,
- requested task scope.

## Claude should validate

### Correctness

Does the implementation actually satisfy the task?

### Simplicity

Is the solution understandable without unnecessary complexity?

### Scope

Did the implementation stay within the requested phase?

### Test confidence

Are the important behaviors covered and passing?

### Maintainability

Can the code be understood and changed later?

### Learning value

For this CNN project, does the implementation remain understandable enough that the code supports learning rather than hiding everything behind abstractions?

---

# Claude Final Verdict

Claude should return one:

```text
APPROVED
APPROVED WITH MINOR NOTES
CHANGES REQUIRED
```

The ideal completion state is:

```text
Claude: APPROVED
```

or:

```text
Claude: APPROVED WITH MINOR NOTES
```

when the notes are genuinely non-blocking.

If Claude finds a real correctness problem, the work returns to Cursor.

---

# Full Workflow

```text
┌───────────────────────────┐
│ 1. Define one small task  │
└─────────────┬─────────────┘
              ↓
┌───────────────────────────┐
│ 2. Cursor implements      │
└─────────────┬─────────────┘
              ↓
┌───────────────────────────┐
│ 3. Codex tests + reviews  │
└─────────────┬─────────────┘
              ↓
        Issues found?
          /       \
        YES       NO
        ↓          ↓
┌──────────────┐   │
│ Cursor fixes │   │
└──────┬───────┘   │
       ↓           │
┌──────────────┐   │
│ Codex retest │───┘
└──────┬───────┘
       ↓
┌───────────────────────────┐
│ 4. Claude final validates │
└─────────────┬─────────────┘
              ↓
        Final approved?
          /       \
        NO        YES
        ↓          ↓
   Cursor fixes   DONE
```

---

# Responsibility Matrix

| Responsibility | Cursor | Codex | Claude |
|---|---|---|---|
| Main implementation | ✅ | ❌ | ❌ |
| Modify production code | ✅ | Only when specifically requested | Normally ❌ |
| Run tests | ✅ | ✅ | Verify/re-run when useful |
| Add missing review tests | As part of implementation | ✅ | Normally ❌ |
| Technical code review | Self-check | ✅ Primary | ✅ Final |
| Find regressions | Basic | ✅ Primary | ✅ Final check |
| Check over-engineering | Basic | ✅ | ✅ |
| Validate task scope | ✅ | ✅ | ✅ |
| Final approval | ❌ | Technical approval | ✅ Final validator |

---

# Working Rule

For every task, use this order:

```text
IMPLEMENT → VERIFY → VALIDATE
```

which maps to:

```text
Cursor → Codex → Claude
```

Do not use all three tools to independently implement the same task.

That creates unnecessary duplication and makes it harder to know which implementation is authoritative.

---

# Small-Step Rule for This CNN Project

The CNN project is being built as a learning project.

Therefore, work on **one study checkpoint at a time**.

Example:

```text
Step 1: CIFAR-10 basics
        ↓
Cursor implements only the supporting code/docs needed for Step 1
        ↓
Codex tests/reviews
        ↓
Claude validates
        ↓
Step 1 complete
        ↓
Move to Step 2
```

Do not ask Cursor to build the entire CNN project before the corresponding concepts have been studied.

---

# Git Rule

Prefer one clean commit for each approved checkpoint.

Example:

```text
study/step-01-cifar10-basics
study/step-02-first-conv-layer
study/step-03-output-shape
```

Suggested cycle:

```text
create branch
→ Cursor implementation
→ Codex verification
→ fixes
→ Claude validation
→ commit / merge
```

---

# Definition of Done

A task is complete only when:

```text
[ ] Cursor implemented the requested scope
[ ] Relevant tests were added/updated
[ ] Cursor's tests pass
[ ] Codex independently reviewed the code
[ ] Codex independently ran relevant tests
[ ] All BLOCKER issues are resolved
[ ] All MAJOR issues are resolved
[ ] Claude performed final validation
[ ] Claude returned APPROVED or APPROVED WITH MINOR NOTES
```

Only then move to the next CNN study checkpoint.
