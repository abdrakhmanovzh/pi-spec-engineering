# Intent format

Use this structure for `intent.md`. Omit an optional section when it adds no information. Do not add technical design details.

```markdown
# Intent: <short outcome-oriented title>

## Objective

Why this change is wanted and what successful outcome it should create.

## Actors and outcomes

- **<Actor>:** <outcome the actor needs>

## Agreed behavior

1. <Observable behavior or business capability>
2. <Observable behavior or business capability>

## Business rules and constraints

- <Rule or constraint that must remain true>

## Scenarios

### <Scenario name>

- **Given:** <starting situation>
- **When:** <event or actor action>
- **Then:** <observable outcome>

## Out of scope

- <Explicitly excluded outcome or behavior>
```

## Quality criteria

An intent is ready when:

- its objective is expressed as an outcome, not a solution
- actors and expected outcomes are unambiguous
- behavior is observable from a business or user perspective
- relevant business rules, boundaries, and unhappy paths are represented
- exclusions make the scope boundary clear
- terminology is consistent
- no statement silently resolves a contradiction in the source
- it contains no implementation design
- the user has explicitly approved it
