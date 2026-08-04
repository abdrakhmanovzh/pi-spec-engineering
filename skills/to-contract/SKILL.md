---
name: to-contract
description: Turn an approved intent document into a technical, verifiable contract through codebase investigation and focused technical decisions. Use before implementing a change whose boundaries, types, behavior, or failure semantics need to be explicit.
disable-model-invocation: true
---

# To Contract

Turn the approved business intent in `intent.md` into an approved technical agreement in `contract.md`.

## Artifact model

- **Requirements** are the original business request in `requirements.md`.
- **Intent** is the clarified and human-approved business outcome in `intent.md`.
- **Contract** is the technical, verifiable agreement derived from the intent.

This skill transforms `intent.md` into `contract.md`. It must not modify either `requirements.md` or `intent.md`.

Read [references/CONTRACT-FORMAT.md](references/CONTRACT-FORMAT.md) before beginning.

## Input

The user must supply an `intent.md`. If its path is not provided, ask for it.

Read it completely and treat it as the authoritative source of desired business behavior.

The intent must be a self-contained handoff. Do not read `requirements.md` to supplement or reinterpret it. If the intent is incomplete, contradictory, or too ambiguous to contract without guessing, stop and report what must be clarified with `/to-intent`. Wait for the user rather than changing the intent.

## Process

### 1. Establish the implementation context

Determine whether the work targets an existing system or is greenfield. Infer this from the intent and available repository; ask only when unclear.

- **Existing system:** inspect the relevant implementation. If it is unavailable, stop rather than inventing it.
- **Greenfield:** confirm that no implementation exists, then define only the minimum new public boundary required by the intent. Treat every concrete detail as newly agreed.

Never silently treat an unavailable existing system as greenfield.

For an available existing system, inspect the relevant code, tests, schemas, APIs, events, documentation, and repository instructions. Identify:

- existing public boundaries affected by the intent
- current input and output types
- existing behavioral and error conventions
- persisted state and external side effects
- existing test seams and verification commands
- technical constraints that affect scope or behavior

Prefer existing boundaries and conventions. Do not introduce a new boundary merely to make the contract look cleaner.

Treat every concrete endpoint, type, identifier format, status code, and platform capability as a claim requiring provenance. Include it only when it comes from inspected repository evidence or an explicit user decision. Never describe a boundary as existing when that status has not been established.

If there is no existing codebase, establish only the minimum technical context needed to define the first public boundary. Treat concrete details as new decisions, not existing facts.

### 2. Detect intent conflicts

Surface any point where technical reality makes the approved intent impossible, unsafe, or materially different in cost or behavior.

Do not repair, reinterpret, or narrow the intent silently. Stop and ask the user whether to revise the intent before continuing. Changes to business meaning belong in `intent.md`, not in the contract.

### 3. Resolve technical decisions

Interview the user only about decisions required to make the contract precise. Walk the decision tree in dependency order.

For every turn:

1. Ask exactly one focused technical decision question.
2. Explain briefly why it affects the contract.
3. Provide your recommended answer and the reason for it, grounded in the existing system where possible.
4. Wait for the user's answer before continuing.

Maintain a private decision ledger. Never ask for a fact that can be established from the repository, and never repeat an answered decision in different words. Revisit a decision only when new information conflicts with it, and explain the conflict.

Focus on:

- public boundaries
- exact input and output types
- validation and normalization
- errors and failure semantics
- invariants
- state transitions
- externally observable side effects
- ordering, idempotency, concurrency, and retry behavior when relevant
- confirmed success, confirmed rejection, and ambiguous outcomes for remote operations
- compatibility constraints explicitly required by the intent or repository
- evidence that can verify each behavior

Do not ask the user to design internal modules, files, classes, helper functions, or implementation steps.

### 4. Draft the contract

Draft `contract.md` using the required format.

The contract must:

- cover every behavior and constraint in the intent
- define concrete inputs and outputs at affected public boundaries
- use native types or established schema formats when they express behavior more precisely than prose
- distinguish existing boundaries from new or changed behavior
- define relevant errors, invariants, state changes, and side effects
- give every verifiable behavior a stable identifier
- map every behavior to prospective automated or manual evidence
- state technical exclusions explicitly

The contract must not contain file-by-file edits, private implementation structure, task sequencing, estimates, or speculative extensibility. For each type, identify who outside the implementation consumes it. Remove it when there is no external consumer and it does not define an observable protocol.

Do not claim that evidence already exists unless you inspected it. Mark evidence as existing or required.

### 5. Check coverage and consistency

Before presenting the draft, check both directions:

- Every material statement in `intent.md` is represented by one or more contract behaviors or explicitly identified as outside the software boundary.
- Every contract behavior is justified by the intent or by a necessary repository constraint.

Then stress-test the contract:

- Does an error actually prove that an operation did not occur?
- Can a side effect succeed while its response is lost?
- Does the contract confuse actual state with client-confirmed state?
- Do retries require idempotency?
- Can all behaviors and invariants hold under timeout, concurrency, and partial failure?
- Does the proposed evidence genuinely prove each behavior?
- Is every concrete technical fact grounded in inspected evidence or an explicit decision?
- Did any private implementation state leak into the contract?

For remote operations, distinguish confirmed success, confirmed rejection, and ambiguous outcome unless the established protocol eliminates ambiguity.

Remove scope creep and resolve contradictions. Surface any uncovered intent instead of guessing.

### 6. Confirm and write

Show the complete contract draft to the user and ask whether it accurately represents the technical agreement. Incorporate corrections and repeat confirmation when meaning changes.

Only after explicit approval, write `contract.md` beside `intent.md` unless the user specifies another location. If `contract.md` already exists, ask before replacing it.

Report the written path. Do not implement code, create tickets, or modify the approved intent as part of this skill.
