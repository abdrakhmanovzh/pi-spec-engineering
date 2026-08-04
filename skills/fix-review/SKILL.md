---
name: fix-review
description: Resolve structured CONTRACT-### and CODE-### review findings with focused code changes and validation. Use after review reports changes required.
disable-model-invocation: true
---

# Fix Review

Resolve findings from `/review` without changing the approved contract.

## Artifact model

- **Contract** is the approved technical agreement.
- **Review findings** identify contract or code problems by stable ID.
- **Fix Review** changes the implementation and records how each finding was resolved.

This skill uses `contract.md`, the reviewed implementation, and the latest review report. Do not read `intent.md` or `requirements.md` to reinterpret the contract.

## Input

Use the review report already present in the conversation or a report supplied by the user. The report must contain stable `CONTRACT-###` or `CODE-###` finding IDs.

Use the same `contract.md` and implementation comparison established by `/review`. Ask for either when it is unavailable or ambiguous.

Read every finding, the cited code, relevant surrounding code, and the contract behavior or invariant referenced by contract findings.

## Process

### 1. Validate the findings

Do not blindly accept every finding. For each one, determine whether it is:

- **Accepted:** the finding is correct and can be fixed without changing the contract.
- **Rejected:** repository evidence shows the finding is incorrect, already resolved, or unrelated to the reviewed change.
- **Blocked:** resolving it requires a contract decision, unavailable dependency, or action outside the repository.

Explain rejected and blocked findings with concrete evidence. Never reject a finding merely because the fix is inconvenient.

If a finding exposes a bad or incomplete contract, mark it blocked and report what must be reconsidered with `/to-contract`. Wait for the user; do not silently redefine expected behavior in code.

### 2. Plan the fix order

Resolve accepted findings in dependency order, prioritizing blocking, high, and medium severity. Combine fixes only when they affect the same behavior or code path.

Keep the change focused. Do not perform unrelated cleanup, speculative refactoring, or additional product work.

### 3. Fix and validate

For each accepted finding:

1. Reproduce or establish the reported problem when practical.
2. Make the smallest clear correction that preserves the contract.
3. Add or strengthen evidence when the finding exposed a coverage gap.
4. Run the narrowest relevant validation.
5. Check that the fix does not regress other contract behaviors or accepted findings.

After all accepted findings are addressed, run the repository checks relevant to the complete changed area.

Do not claim a finding is fixed only because code changed. Require passing evidence or mark it blocked when validation cannot be completed.

### 4. Report

Report every original finding ID exactly once under one status:

```markdown
## Fixed

- **CONTRACT-001:** <change made and evidence that now passes>

## Rejected

- **CODE-002:** <why the finding is incorrect, with repository evidence>

## Blocked

- **CONTRACT-003:** <decision or dependency required>

## Validation

- `<command>` — pass | fail | not run: <reason>

## Result

Ready for review | Blocked
```

Omit empty status sections. Report **Ready for review** only when all accepted findings are fixed with evidence and no finding is blocked.

Do not modify `contract.md`, commit changes, publish results, close findings permanently, or declare the implementation verified. Recommend running `/review` again with the changed implementation and resolution report.
