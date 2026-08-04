---
name: verify
description: Independently verify that an implemented and reviewed change delivers its approved intent through the technical contract and working-system evidence. Use after review passes.
disable-model-invocation: true
---

# Verify

Verify the completed change from the approved business intent through observable system behavior.

## Artifact model

- **Intent** is the approved business outcome and behavior.
- **Contract** is the technical agreement derived from the intent.
- **Implementation** is the reviewed code and evidence intended to realize the contract.
- **Verification** demonstrates that the complete change delivers the intent.

This skill checks `intent.md` against `contract.md` and the working system. Do not read `requirements.md`; it is unrefined historical input, not the approved verification target.

Read [references/VERIFY-FORMAT.md](references/VERIFY-FORMAT.md) before beginning.

## Input

The user must supply both `intent.md` and `contract.md`. Ask for either path when missing.

Use the final implementation and latest `/review` report from the current repository and conversation, or ask the user to supply the review report when unavailable. Proceed only when the review result is **Pass**.

Recreate the comparison described in the review report. Confirm that its complete changed-path set matches the reviewed-state manifest, including untracked files, and that every listed path still has the recorded status, mode, and SHA-256 of its current bytes. Confirm the current contract SHA-256 too. If anything differs, stop and recommend another `/review`; the passing review does not cover the current state.

Read the complete intent and contract, repository instructions, relevant tests and public boundaries, and the commands needed to exercise the finished behavior.

## Independence

When the harness supports subagents, delegate the complete verification to one isolated, read-only subagent. Give it the intent, contract, latest review report, repository instructions, and access to run verification commands. It must inspect evidence itself rather than trust implementation or review summaries.

When subagents are unavailable, perform the same work in the primary session. Independence is preferred, not required for portability.

## Process

### 1. Check that the contract matches the intent

Compare the approved intent with the technical contract.

Confirm that:

- every agreed behavior, scenario, business rule, constraint, and exclusion is represented
- the contract does not narrow or alter business meaning
- technical behavior not present in the intent is necessary to deliver it rather than added product scope
- outcomes outside the software boundary are identified honestly

If the contract omits, changes, or contradicts the intent, stop and report a contract mismatch. State exactly what must be reconsidered with `/to-contract`; do not invoke it or change either artifact automatically, because revising the contract requires user approval.

### 2. Build the verification map

For every software-verifiable intent behavior, scenario, business rule, and constraint, identify:

- the contract behaviors and invariants that realize it
- the observable boundary where it can be demonstrated
- the automated or manual evidence that proves it

Evidence may include end-to-end tests, integration tests, public-interface tests, schema or type checks, builds, runtime inspection, or a documented manual demonstration. Prefer the highest practical observable boundary.

Passing commands alone are insufficient when they do not exercise the intended outcome.

### 3. Run the evidence

Run the relevant verification commands and demonstrations against the final implementation.

For each software-verifiable intent item, record:

- exact evidence used
- command or procedure
- observed result
- pass, fail, or blocked status

Do not claim business metrics outside the software boundary, such as reduced support volume or increased conversion, were achieved by implementation. Verify only that the system capability needed to pursue them exists.

### 4. Classify failures

Assign stable `VERIFY-###` identifiers to failures and route them by cause:

- **Contract mismatch:** state what must be reconsidered with `/to-contract` and wait for the user.
- **Implementation or missing-evidence problem:** state that `/fix-review` and then `/review` are required.
- **Unavailable environment or external dependency:** mark blocked and state what is needed.

Do not fix code, modify artifacts, or weaken acceptance criteria during verification.

### 5. Report

Use the required verification format. Report every intent behavior, scenario, business rule, and constraint, including those outside the software boundary.

The result is:

- **Pass:** every software-verifiable intent item passes and no verification failure is open.
- **Fail:** at least one intent, contract, implementation, or evidence failure exists.
- **Blocked:** required verification cannot be completed because an environment or dependency is unavailable.

On Pass, include a concise completion package for `/finalize`:

- delivered business behavior
- verification commands and results
- outcomes outside the software boundary
- durable truths that should remain in code, tests, schemas, or documentation

Do not publish the completion package, delete working artifacts, modify code, or commit changes as part of this skill.
