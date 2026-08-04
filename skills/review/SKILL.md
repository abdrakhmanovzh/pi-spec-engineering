---
name: review
description: Review an implementation diff independently against an approved contract and repository engineering standards. Use after implementation to produce structured, actionable findings without modifying code.
disable-model-invocation: true
---

# Review

Review an implementation against `contract.md` without modifying it.

## Artifact model

- **Contract** is the self-contained technical agreement.
- **Implementation** is the code and evidence intended to satisfy it.
- **Review** evaluates the contract and the code as separate checks.

This skill consumes only `contract.md`, the implementation diff, and repository context. Do not read `intent.md` or `requirements.md` to reinterpret the contract.

Read [references/REVIEW-FORMAT.md](references/REVIEW-FORMAT.md) before beginning.

## Input

The user must supply a `contract.md`. If its path is not provided, ask for it.

Determine the implementation diff:

- For uncommitted work, include staged, unstaged, and untracked files.
- For committed work, use the comparison point supplied by the user.
- If the intended comparison point is ambiguous, ask before reviewing.

Read the contract, repository instructions, complete diff, changed files, and enough surrounding code and tests to evaluate behavior accurately.

Create one exact review patch containing every in-scope staged, unstaged, untracked, or committed change. Give both reviewers this same patch.

Record a reproducible reviewed-state manifest: the full base revision, full target revision or `working tree`, and every changed path in sorted order with its status, file mode, and SHA-256 of its current bytes. Use `DELETED` instead of a hash for deleted paths. The manifest path set must exactly match the patch. Record the contract path and SHA-256 separately.

## Process

### 1. Validate the review target

Confirm that the contract is readable, the comparison point resolves when applicable, and the implementation diff is non-empty. Confirm that the review patch includes untracked in-scope files. Report and stop when there is nothing reliable to review.

Treat repository-generated files and unrelated pre-existing failures separately from implementation changes.

### 2. Run independent review axes

When the harness supports subagents, run Contract Review and Code Review in parallel, isolated, read-only subagents. Give each the contract, exact review patch, reviewed-state manifest, repository instructions, and its check instructions below. Neither reviewer may modify files or see the other review before reporting.

When subagents are unavailable, review the axes sequentially and keep their analysis and findings separate. Subagents improve independence but are not required for this skill to work.

#### Contract review

Evaluate every contract behavior, invariant, type, error, side effect, and required evidence against the implementation.

Look for:

- missing or partially implemented behaviors
- behavior that contradicts the contract
- incorrect boundary types, validation, errors, or state transitions
- failure, timeout, concurrency, retry, or partial-success behavior that violates the contract
- required evidence that is absent, weak, or does not prove its behavior
- behavior added without contractual justification

Do not infer compliance from names, comments, or passing tests alone. Trace the relevant execution path and check whether the evidence can fail when behavior is wrong.

#### Code review

Evaluate the changed implementation independently of contract completeness.

Look for concrete defects and regressions involving:

- correctness and edge cases
- security and privacy
- reliability and data integrity
- concurrency and resource lifecycle
- error handling and observability
- performance when materially affected by the diff
- repository conventions and documented standards
- unnecessary complexity, duplication, or speculative abstraction introduced by the change

Do not report purely subjective preferences, issues enforced by existing tooling, or unrelated pre-existing problems. A contract-compliant implementation can still fail this axis.

### 3. Validate findings

Aggregate the two independent reports without letting one axis suppress or rerank the other. Before reporting a finding:

- verify it against the actual changed code and surrounding context
- distinguish demonstrated defects from plausible but unproven risks
- identify the affected contract identifier when applicable
- cite the smallest useful file and line location
- explain the observable consequence
- propose the smallest contract-preserving resolution

Do not invent findings to fill severity levels. If an axis has no findings, say so.

### 4. Report

Use the required review format.

Assign stable identifiers:

- `CONTRACT-###` for Contract Review findings
- `CODE-###` for Code Review findings

Order findings by severity within each axis, but never merge the axes into one ranking. Use severities consistently:

- **Blocking:** unsafe to proceed; the implementation cannot satisfy the contract or safely ship.
- **High:** likely material incorrectness, security, data loss, or major missing behavior.
- **Medium:** real defect or maintainability problem with limited impact.
- **Low:** small actionable issue worth fixing; never use for style preference.

End with counts by axis and severity plus a clear review result:

- **Pass:** no findings.
- **Changes required:** one or more findings exist.

Do not modify code, tests, the contract, git state, or issue trackers as part of this skill.
