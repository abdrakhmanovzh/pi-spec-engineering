# Verification format

Use this structure and keep evidence concrete.

```markdown
# Verification

## Verified state

- **Contract:** `<path>` — SHA-256: `<hash>`
- **Base:** `<full revision ID>`
- **Target:** `<full revision ID or "working tree">`
- **Implementation manifest:** Matches passing review | Does not match

The changed-path set and every path's status, mode, and content hash must match the passing review report.

## Intent verification

| Intent behavior, scenario, rule, or constraint | Contract coverage | Evidence | Result |
|---|---|---|---|
| <intent item> | B1, I1 | `<command>` or manual procedure and observed result | Pass / Fail / Blocked |

## Failures

### VERIFY-001 — <short title>

- **Cause:** Contract mismatch | Implementation | Evidence | Environment
- **Intent:** <affected behavior, scenario, business rule, or constraint>
- **Contract:** <affected behavior or invariant identifiers>
- **Problem:** <what could not be demonstrated>
- **Next action:** reconsider with `/to-contract` after user approval | run `/fix-review` then `/review` | provide the required environment

If there are no failures: `No verification failures.`

## Outcomes outside the software boundary

- <business outcome that implementation enables but cannot guarantee>

## Completion package

Include this section only when the result is Pass.

- **Delivered:** <concise business behavior>
- **Evidence:** <commands or demonstrations and results>
- **Durable truth:** <code, tests, schemas, or documentation that should remain>

## Result

Pass | Fail | Blocked
```

A passing row must cite evidence that exercises observable behavior. A test name, file path, or successful command without its relevant observed result is not sufficient by itself.
