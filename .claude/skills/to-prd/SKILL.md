# to-prd: Handoff Summary → Product Requirements Document

## Purpose

Convert a PM Handoff Summary (produced by `/grill`) into a full Product Requirements Document. This skill runs in the **PM Agent** on platform.claude.com.

**Input**: PM Handoff Summary block (pasted by user)
**Output**: Structured PRD markdown (displayed in conversation — user saves or posts as needed)

## Before Writing

Read `CONTEXT.md` for domain glossary and role definitions. Reference ADR numbers where decisions are already made.

## PRD Template

```markdown
# PRD: <feature name>

**Issue**: #<number> (if applicable)
**Status**: Draft
**Date**: <date>

---

## Problem Statement

[User-centric. Which user type has this pain, and what is it costing them?]

## Solution

[User-centric description of what we're building. Not implementation — behavior from the user's perspective.]

## User Stories

1. As a [user type], I want [capability], so that [outcome].
2. ...

(Aim for coverage across happy path, edge cases, and error states.)

## Acceptance Criteria

- [ ] [Specific, testable, from the user's perspective]
- [ ] ...

## Implementation Decisions

[Major modules to build or modify. Use "deep module" framing — what encapsulates what.
Reference existing modules by domain name, not file path.
Include interface decisions, data flow, and real-time event handling if relevant.
No specific file paths. Prototype code only when it encodes an architectural decision more precisely than prose.]

## Testing Decisions

[What scenarios tests should cover. What module-level behavior to verify.]

## Out of Scope

- [Explicit exclusions from the grill session]

## Open Questions

- [Items flagged during grill for PM/business decision — must be resolved before implementation starts]

## Further Notes

[Domain constraints, role-gating, architectural context, or other notes worth capturing]
```

## Guidelines

- Use domain terms from `CONTEXT.md` exactly — do not paraphrase
- Reference ADR numbers for decisions already recorded in `docs/adrs/`
- User stories must cover all user types affected where applicable
- Implementation Decisions should name modules by domain — not file paths
- Verify user stories against acceptance criteria — no orphaned stories
- Flag any open questions that block implementation and must be answered before coding starts
