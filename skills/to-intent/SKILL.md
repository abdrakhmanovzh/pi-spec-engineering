---
name: to-intent
description: Turn raw business requirements into an agreed intent document through a rigorous interview. Use when requirements are ambiguous, incomplete, contradictory, or need refinement before creating a technical contract.
disable-model-invocation: true
---

# To Intent

Turn the original business requirements in `requirements.md` into an agreed `intent.md`.

In this workflow:

- **Requirements** are the original business request. They are evidence of what was requested, but may be incomplete, ambiguous, contradictory, or inconsistent with the existing system.
- **Intent** is the clarified business outcome and behavior agreed with the user through this skill.
- **Contract** is the later technical commitment derived from the intent. Producing it is not part of this skill.

Read [references/INTENT-FORMAT.md](references/INTENT-FORMAT.md) before beginning.

## Input

The user must supply a `requirements.md` containing the original business requirements. If its path is not provided, ask for it.

Read it completely, including relevant material it references. Treat it as the original request, not as settled truth.

Never modify or replace `requirements.md`. Clarifications and agreed decisions belong in the separate `intent.md`.

## Process

### 1. Understand the source

Extract:

- the problem being described
- affected actors and their desired outcomes
- requested behavior
- stated business rules and constraints
- examples and edge cases
- explicit exclusions
- assumptions, ambiguities, and contradictions

Distinguish statements present in the source from your own inferences.

### 2. Check relevant reality

When the request concerns an existing system, inspect the relevant code and existing documentation. Surface contradictions between the request, the user's answers, and current behavior. Do not silently choose which is correct.

Do not perform broad codebase exploration when it cannot resolve a requirement question.

### 3. Grill the user

Interview the user until the intent is precise enough to become a technical contract.

- Ask one focused question at a time.
- Prioritize questions whose answers could materially change scope or behavior.
- Use concrete scenarios rather than abstract questions.
- Challenge vague, overloaded, or conflicting terminology.
- Probe unhappy paths, boundaries, permissions, state changes, and partial failure when relevant.
- Do not ask the user to choose implementation details unless a real business constraint depends on them.
- Do not invent an answer merely to complete the document.
- Periodically summarize resolved decisions and remaining uncertainty.

Stop when additional questions would not materially change the agreed behavior, scope, or constraints.

### 4. Draft the intent

Draft `intent.md` using the required format. Keep it concise. Preserve meaningful business rules and examples, but do not turn the document into a transcript.

The intent must describe what outcome and behavior are wanted. It must not prescribe modules, files, APIs, schemas, classes, database design, or implementation sequencing.

Classify open questions as:

- **Blocking** — implementation or contract design would require guessing.
- **Non-blocking** — useful to resolve later, but does not affect the present commitment.

Do not finalize an intent with blocking open questions.

### 5. Confirm and write

Show the complete draft to the user and ask whether it accurately captures the shared intent. Incorporate corrections and repeat confirmation when meaning changes.

Only after explicit approval, write `intent.md` beside `requirements.md` unless the user specifies another location. If `intent.md` already exists, ask before replacing it.

Report the written path and any non-blocking open questions. Do not create a technical contract, implementation plan, tickets, ADRs, or code as part of this skill.
