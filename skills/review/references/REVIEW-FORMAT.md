# Review format

Report findings in this structure. Omit empty severity groupings and keep each finding concise.

```markdown
# Review

## Reviewed state

- **Contract:** `<path>` — SHA-256: `<hash>`
- **Base:** `<full revision ID>`
- **Target:** `<full revision ID or "working tree">`

| Path | Status | Mode | SHA-256 |
|---|---|---|---|
| `<repository-relative path>` | Added / Modified / Deleted | `<file mode>` | `<hash or DELETED>` |

List every path in the exact review patch, including untracked files, sorted by repository-relative path. The path set, status, mode, and hash of current file bytes define the reviewed implementation state.

## Contract review

### CONTRACT-001 — <severity>: <short finding title>

- **Contract:** <behavior or invariant identifiers>
- **Location:** `<file:line>`
- **Problem:** <what the implementation does and how it differs from the contract>
- **Consequence:** <observable impact>
- **Resolution:** <smallest contract-preserving correction>

If there are no findings: `No contract findings.`

## Code review

### CODE-001 — <severity>: <short finding title>

- **Location:** `<file:line>`
- **Problem:** <concrete defect or introduced risk>
- **Consequence:** <observable impact>
- **Resolution:** <smallest appropriate correction>

If there are no findings: `No code findings.`

## Summary

- **Contract review:** <counts by severity>
- **Code review:** <counts by severity>
- **Result:** Pass | Changes required
```

## Finding criteria

A finding must be:

- caused by or exposed through the reviewed change
- supported by concrete code or missing required evidence
- actionable without changing approved business meaning
- specific enough for `/fix-review` to resolve and a later `/review` to close

Do not include praise, general summaries, speculative concerns without a plausible execution path, or formatting preferences as findings.
