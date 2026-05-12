# Architecture Decision Records

An ADR documents a significant architectural decision — what was decided, why, and what the consequences are.

## When to create one

Create an ADR when you make a decision that is:

- **Hard to reverse** — changing it later would require significant rework
- **Non-obvious** — a future reader looking at the code would not infer why this approach was chosen
- **Genuinely a trade-off** — there was a real alternative, and the choice has consequences worth recording

Do not create ADRs for routine decisions (choosing a variable name, picking a common library), or for decisions that are self-evident from the code.

## Format

Create `docs/adrs/ADR-NNN-short-description.md` (e.g. `ADR-001-platform-agent-self-contained-prompts.md`):

```markdown
# ADR-NNN: Title

**Status**: Accepted | Superseded by ADR-XXX
**Date**: YYYY-MM-DD

## Context
What situation or constraint led to this decision?

## Decision
What was decided?

## Consequences
What becomes easier? What becomes harder? What must be maintained as a result?
```

## Index

| ADR | Decision |
|-----|----------|
| *(none yet — add your first ADR when you make a hard architectural decision)* | |
