---
name: finalize
description: Retain the useful history of a verified change, confirm durable system truth, and remove temporary workflow artifacts with explicit approval. Use only after verification passes.
disable-model-invocation: true
---

# Finalize

Complete a verified change without leaving unnecessary workflow artifacts in the repository.

## Artifact model

- **Historical reasoning** belongs in an issue, pull request, or chosen archive.
- **Current system truth** belongs in code, tests, schemas, and durable documentation.
- **Working artifacts** such as `requirements.md`, `intent.md`, and `contract.md` may be removed after useful history and lasting truth are retained.

Read [references/COMPLETION-FORMAT.md](references/COMPLETION-FORMAT.md) before beginning.

## Input

Use the latest `/verify` report and completion package from the conversation or a report supplied by the user. Do not proceed unless its result is **Pass**.

Require the latest passing `/review` report from the conversation or user. Confirm that its contract hash and reviewed-state manifest match the passing verification report and the current repository state. If they differ, stop; review and verification do not cover the current change.

Identify the exact working artifact paths from the conversation or ask the user for them. Never infer cleanup targets from filenames alone.

## Process

### 1. Confirm readiness

Inspect the final repository state and successful verification evidence. Confirm that lasting truths needed after this change are represented in their canonical locations, such as:

- public types and schemas
- automated tests
- API, user, or operator documentation
- business and security constraints enforced by the system
- an ADR only for a durable, surprising, hard-to-reverse trade-off

If important current truth exists only in a temporary workflow artifact, stop and report what must be promoted. Do not implement or document it as part of this skill; recommend the appropriate earlier workflow stage and another review and verification pass.

### 2. Prepare the completion record

Create a concise, ready-to-paste record using the required format. Derive it from the passing verification package, passing review, and final repository state.

Include historical information useful to a future reader. Do not copy entire requirements, intent, contract, review, or verification documents into it.

### 3. Choose retention

Show the completion record and ask the user where it will be retained:

- originating issue
- pull request
- local archive
- another durable destination
- nowhere, accepting intentional loss of the history

This initial skill does not publish to remote trackers. The user must paste the record and confirm that it was retained. If the user chooses a local archive, ask for the exact path and approval before writing it.

Do not claim publication occurred merely because the record was generated.

### 4. Propose cleanup

After retention is confirmed or the user explicitly accepts losing the history, list each temporary artifact proposed for deletion with its exact path and why it is temporary.

Never propose deleting:

- source code or tests
- public schemas or types
- durable product, API, or operator documentation
- repository instructions
- an artifact whose lasting information has not been retained elsewhere

Do not use directory-wide or wildcard deletion. Ask for explicit approval of the listed paths.

### 5. Clean up and report

Delete only the exact files the user approved. Leave declined files untouched. Do not delete parent directories unless the user explicitly approves them and they are empty.

Report:

- where the completion record was retained, or that history loss was accepted
- durable truths confirmed in the repository
- files deleted
- files retained
- final repository status

Do not commit, push, open or modify tracker items, or declare publication that the user did not confirm.
