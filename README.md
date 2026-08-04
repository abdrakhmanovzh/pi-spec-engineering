# Agentic Engineering Skills

A small, composable workflow for turning business requirements into verified software without adopting a heavyweight spec framework.

The repository is intentionally built as a collection of independent agent skills. Each skill owns one transition in the workflow and can be adapted or used separately.

## Workflow

```text
requirements.md
    ↓ /to-intent
intent.md
    ↓ /to-contract
contract.md
    ↓ /implement
implementation
    ↓ /review ⇄ /fix-review
reviewed implementation
    ↓ /verify
verified change
```

Only `/to-intent` exists today. The rest describe the direction of the project, not implemented functionality.

## Artifact model

### `requirements.md`

The original business request supplied by the user. It may be incomplete, ambiguous, contradictory, or inconsistent with the existing system. It is preserved unchanged as source material.

### `intent.md`

The clarified and human-approved business outcome produced through grilling. It captures actors, observable behavior, business rules, scenarios, constraints, and explicit exclusions without prescribing technical implementation.

### `contract.md`

The technical and verifiable commitment derived from `intent.md`. It will define boundaries such as inputs, outputs, types, errors, invariants, state transitions, and observable side effects without becoming an implementation plan.

## Principles

- Business intent is clarified before technical commitments are made.
- Agents investigate discoverable facts and ask humans for decisions.
- Humans approve meaning; agents implement and provide evidence.
- Artifacts describe boundaries and outcomes, not speculative internals.
- Skills remain independently understandable and explicitly invoked.
- Workflow depth should be proportional to the uncertainty and risk of a change.
- Tests, types, and runtime checks provide executable evidence for written commitments.

## Available skills

### `/to-intent`

Transforms a supplied `requirements.md` into a separate, approved `intent.md` through a focused interview.

It:

- reads and preserves the original requirements
- inspects relevant code and documentation when facts can be discovered there
- walks decisions in dependency order
- asks one decision question at a time
- provides a recommended answer and rationale
- avoids repeating resolved questions
- produces no technical design, contract, tickets, or implementation
- shows the complete intent for approval before writing it

By default, `intent.md` is written beside the supplied `requirements.md`:

```text
changes/order-tracking/
├── requirements.md
└── intent.md
```

The user may request another output location. Existing intent files are not replaced without confirmation.

## Using `/to-intent` with Pi

Load the development version directly:

```bash
pi --skill ./skills/to-intent/SKILL.md
```

Then invoke it explicitly in the fresh session:

```text
/skill:to-intent path/to/requirements.md
```

The skill uses `disable-model-invocation: true`, so it will not be selected automatically.

## Development

Skills live under `skills/` so this repository can distribute them without coupling the source layout to a specific agent harness:

```text
skills/
└── to-intent/
    ├── SKILL.md
    └── references/
        └── INTENT-FORMAT.md
```

Evaluate skills in fresh sessions using realistic, imperfect inputs. Judge behavior and artifact quality rather than exact wording.
