# Completion record format

Produce a concise record suitable for an issue, pull request, or local archive.

```markdown
## Change completed

### Delivered

- <business behavior now available>

### Important decisions

- <scope or technical decision worth retaining historically>

Omit this section when no decision needs historical explanation.

### Evidence

- `<command or procedure>` — <observed passing result>

### Review

- Contract Review: <result and finding count>
- Code Review: <result and finding count>

### Outside the software boundary

- <business outcome enabled but not guaranteed by implementation>

Omit this section when none exists.

### Durable truth

- <canonical code, test, schema, or documentation location>
```

Do not include temporary file paths as durable truth. Do not paste full workflow artifacts or verbose implementation summaries.
