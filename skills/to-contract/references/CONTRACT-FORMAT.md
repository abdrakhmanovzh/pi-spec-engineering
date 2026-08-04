# Contract format

Use this structure for `contract.md`. Omit a section when it is irrelevant. Prefer precise types, tables, and short rules over long prose.

````markdown
# Contract: <short title>

## Scope

A concise statement of the exact capability this implementation commits to deliver.

## Boundaries

### `<boundary name>`

- **Kind:** API | command | event | user interface | job | library interface | other
- **Status:** existing | changed | new
- **Responsibility:** <what is observable at this boundary>
- **Input:** <exact public type or "None">
- **Output:** <exact public type or "None">
- **Errors:** <exact observable error type or "None">

Use the repository's native language or established schema notation for each type. State validation and normalization rules that the notation cannot express. Include only types that cross this boundary or define its externally observable protocol; omit private UI and implementation state.

Repeat for every affected boundary. Name shared types explicitly where they are reused.

## Behaviors

### B1 — <short behavior name>

- **Given:** <relevant starting state or input>
- **When:** <operation or event>
- **Then:** <observable output or state>
- **Errors:** <observable failure behavior, or "None specific">
- **Side effects:** <observable effects and ordering, or "None">

Repeat with stable sequential identifiers (`B2`, `B3`, ...).

## Invariants

- **I1:** <condition that must always hold>

## Evidence

| Behavior | Evidence | Status |
|---|---|---|
| B1 | <test seam, check, or manual demonstration> | Existing / Required |

Include invariants in the Behavior column when they require separate evidence.

## Intent coverage

| Intent section or item | Contract coverage |
|---|---|
| <intent behavior or constraint> | B1, I1 |

Mark outcomes outside the software boundary explicitly and explain why they cannot be guaranteed by implementation.

## Out of scope

- <explicit technical exclusion>
````

## Quality criteria

A contract is ready when:

- its source intent was approved
- the work is identified as existing-system or greenfield
- an unavailable existing system was not treated as greenfield
- scope is exact rather than conditional or aspirational
- affected public boundaries are named
- inputs, outputs, and errors have concrete types where applicable
- every concrete technical fact comes from inspected evidence or an explicit user decision
- validation and normalization are explicit
- observable behavior has stable identifiers
- private implementation types and state are absent
- relevant failure, retry, idempotency, ordering, and partial-success semantics are defined
- remote operations distinguish confirmed success, confirmed rejection, and ambiguous outcomes when the protocol permits ambiguity
- invariants and side effects are explicit
- every intent behavior is covered or identified as outside the software boundary
- every contract behavior is justified by intent or a necessary repository constraint
- evidence is identified without pretending required evidence already exists
- behaviors and invariants remain consistent under timeout, concurrency, and partial failure
- proposed evidence can actually prove its referenced behaviors
- no private implementation plan or speculative extension is included
- the user has explicitly approved it
