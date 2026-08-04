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

## Types

Use the repository's native language or established schema notation. Include only types that cross a public boundary or define an externally observable protocol. Do not include private UI or implementation state.

```text
<exact input, output, error, event, or state type>
```

State validation and normalization rules that the type notation cannot express.

## Behaviors

### C1 — <short behavior name>

- **Given:** <relevant starting state or input>
- **When:** <operation or event>
- **Then:** <observable output or state>
- **Errors:** <observable failure behavior, or "None specific">
- **Side effects:** <observable effects and ordering, or "None">

Repeat with stable sequential identifiers (`C2`, `C3`, ...).

## Invariants

- **I1:** <condition that must always hold>

## Evidence

| Behavior | Evidence | Status |
|---|---|---|
| C1 | <test seam, check, or manual demonstration> | Existing / Required |

Include invariants in the Behavior column when they require separate evidence.

## Intent coverage

| Intent section or item | Contract coverage |
|---|---|
| <intent behavior or constraint> | C1, I1 |

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
