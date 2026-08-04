---
name: implement
description: Implement an approved contract in an existing or greenfield codebase, including required evidence and validation. Use when contract.md defines the exact technical behavior to build.
disable-model-invocation: true
---

# Implement

Implement the approved technical behavior in `contract.md`.

## Artifact model

- **Intent** is the approved business outcome.
- **Contract** is the self-contained technical agreement derived from that intent.
- **Implementation** is the code, tests, schemas, and documentation that satisfy the contract.

This skill consumes only `contract.md`. Do not read `intent.md` or `requirements.md` to supplement or reinterpret it.

## Input

The user must supply a `contract.md`. If its path is not provided, ask for it.

Read it completely, then read the repository instructions and inspect the affected code and test conventions.

## Process

### 1. Validate the handoff

Before editing, confirm that:

- the scope and affected boundaries are identifiable
- each behavior is implementable without guessing about observable results
- required types, errors, invariants, and side effects are sufficiently precise
- behaviors and evidence do not contradict each other
- the repository is consistent with the contract's claims about existing behavior

If the contract is incomplete, contradictory, impossible, or inconsistent with the repository, stop and report what must be reconsidered with `/to-contract`. Wait for the user; do not repair the contract or infer replacement behavior.

### 2. Establish the baseline

Identify the smallest relevant validation commands and run enough of them to distinguish pre-existing failures from regressions. Inspect existing tests at the contract's public boundaries before creating new seams.

Do not perform unrelated cleanup or broad exploration.

### 3. Implement in vertical slices

Implement one observable behavior at a time through the affected layers. Use the contract behavior identifiers to track coverage privately.

For each slice:

1. Select one or a tightly coupled set of contract behaviors.
2. Add or update the required evidence at the highest practical public boundary.
3. Implement only enough production behavior to satisfy that slice.
4. Run the focused test, typecheck, or other relevant check.
5. Refactor only when needed to keep the implementation clear and consistent with repository conventions.

Use test-first development when it provides a useful feedback loop, especially for behavior and bug fixes. Do not force TDD when the evidence is a schema check, static type, build result, or manual demonstration.

Prefer existing public boundaries and test seams. Do not expose private internals solely to make them testable.

### 4. Preserve the contract

- Implement every in-scope behavior and invariant.
- Materialize contract types in the repository's native schema or type system when applicable.
- Preserve exact externally observable values when the contract specifies them.
- Do not add behavior merely because it seems useful.
- Do not silently narrow, extend, or reinterpret the contract.

If implementation reveals a necessary contract change, stop. Report the affected behavior identifiers, the repository evidence, and the decision required from `/to-contract`.

### 5. Validate

Run focused checks throughout implementation. At the end, run all repository checks relevant to the changed area, such as tests, typechecking, linting, building, schema validation, or a documented manual demonstration.

Distinguish:

- passing evidence
- failures introduced by the change
- confirmed pre-existing failures
- evidence that could not be run

Do not claim a behavior is satisfied without evidence.

### 6. Report

Report concisely:

- contract behaviors implemented
- code and durable documentation changed
- evidence added or reused for each behavior
- validation commands and results
- deviations, unverified behaviors, or remaining failures

Do not create another planning or completion artifact unless the user asks. Do not review the implementation, modify the contract, commit changes, or publish results as part of this skill.
