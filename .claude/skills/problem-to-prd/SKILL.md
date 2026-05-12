# problem-to-prd: Raw Problem → Product Requirements Document

## Purpose

Convert a raw problem description, bug report, or feature request directly into a full PRD — skipping the `/grill` interview. Use this when the problem is already well-understood and a deep design interview would be overkill.

**Input**: Free-form text — bug report, Slack message, GitHub issue body, feature description, etc.
**Output**: Structured PRD markdown in the same format as `/to-prd`.

## When to use this skill vs. `/grill` + `/to-prd`

| Situation | Use |
|-----------|-----|
| Bug with clear current vs. expected behavior | `/problem-to-prd` |
| Small, well-scoped feature affecting one user type | `/problem-to-prd` |
| Re-stating an existing GitHub issue as a PRD | `/problem-to-prd` |
| Vague idea, multiple unknowns, cross-role impact | `/grill` then `/to-prd` |
| Architectural decisions still open | `/grill` then `/to-prd` |

If during inference you discover the problem is too ambiguous for 3 questions to resolve, stop and recommend `/grill` instead.

## Before Writing

Read these to ground the PRD in domain truth:
- `CONTEXT.md` — domain glossary, role definitions, key constraints
- `docs/adrs/` index — existing architecture decisions; open any that the problem touches
- `.claude/rules/structure.md` — where code lives (so Implementation Decisions reference real modules)

Load additional rule files when the problem touches those areas (see the table in `CLAUDE.md`).

If a GitHub issue number or URL is referenced, fetch it:
- Bare number `#123` → assume current repo (`TODO: your-org/your-repo`)
- Full URL → extract org, repo, and number
- Fetch: `gh issue view <number> --repo <org>/<repo> --json title,body,labels,assignees,comments`

## Workflow

### Step 1: Extract what's already there

Read the raw input and pull out everything that's already explicit. For each PRD section, note whether it's **stated**, **inferable**, or **missing**.

| PRD Section | What to look for in raw input |
|-------------|-------------------------------|
| Problem Statement | Pain points, "users can't…", "currently broken because…" |
| User type | Mentions of specific roles, UI areas, or workflows |
| Current behavior | "Right now…", screenshots, error messages |
| Expected behavior | "Should…", "want to…", "instead of…" |
| Scope hints | "Just for X", "not changing Y", explicit non-goals |
| Acceptance shape | Steps to reproduce, success conditions |
| Affected modules | Component names, route paths, service references |

### Step 2: Auto-detect user type

Infer the affected user type(s) from the input. Use the role definitions in `CONTEXT.md` as your reference. If no clear signal → ask as one of the 3 questions.

### Step 3: Decide whether to ask

After extraction, count critical gaps. A gap is critical if it blocks writing a specific, testable acceptance criterion or a defensible Implementation Decision.

- **Zero critical gaps** → skip questions, generate PRD directly. Mark any inferred-but-unverified facts in **Open Questions** with the prefix `Assumed:` so the human can confirm.
- **1–3 critical gaps** → ask focused questions, one block, all at once. Wait for answers before generating.
- **More than 3 critical gaps** → stop. Output: *"This problem has too many open dimensions for problem-to-prd. Recommend running `/grill` first to resolve the design decisions."* Then list the gaps so the user can decide.

### Step 4: Ask focused questions (when needed)

Rules:
- **Maximum 3 questions.** Pick the highest-leverage gaps — ones that change the PRD shape, not minor wording.
- **Ask in a single message**, numbered. Do not drip them out.
- **Provide a recommended answer** with each question so the user can accept by saying "yes" or correct quickly.
- Prefer questions that constrain scope ("Should this also affect X?") over open-ended ones ("How should this work?").

Good question shapes:
- "Should this apply to active records only, or also to archived ones? (Recommend: active only — archived records are immutable.)"
- "Is this for user type A only, or does user type B need it too? (Recommend: A only based on the input.)"

Bad question shapes:
- "What do you want?" (too open)
- "Should the button be blue or green?" (too granular)
- "Is this important?" (won't change the PRD)

### Step 5: Generate the PRD

Use the template below. Every claim must be either (a) explicit in the input, (b) confirmed via question, or (c) flagged in Open Questions as `Assumed:`.

## PRD Template

```markdown
# PRD: <feature name>

**Issue**: #<number> (if applicable)
**Status**: Draft
**Date**: <date>
**Source**: problem-to-prd (raw input — no grill session)

---

## Problem Statement

[User-centric. Which user type has this pain, and what is it costing them?]

## Solution

[User-centric description of what we're building. Not implementation — behavior from the user's perspective.]

## User Stories

1. As a [user type], I want [capability], so that [outcome].
2. ...

(Cover happy path, edge cases, and error states.)

## Acceptance Criteria

- [ ] [Specific, testable, from the user's perspective]
- [ ] ...

## Implementation Decisions

[Major modules to build or modify. Use "deep module" framing — what encapsulates what.
Reference existing modules by domain name, not file path.
Include interface decisions, data flow, and real-time event handling if relevant.
No specific file paths. Prototype code only when it encodes an architectural decision more precisely than prose.]

## Testing Decisions

[What scenarios tests should cover.]

## Success Metrics

[How we know this worked. Skip for narrow bug fixes. Include for features where outcome is measurable.

Primary metric — the one that decides whether to keep, iterate, or revert.
Guardrail metrics — what would tell us we made things worse.]

## Out of Scope

- [Explicit exclusions inferred from input or confirmed via questions]

## Open Questions

- [Items that need PM/business resolution before implementation starts]
- Assumed: [Inferences made without confirmation — flag these so the human can verify]

## Further Notes

[Domain constraints, role-gating, ADR references.

Rollout — when the change is risky or needs a backfill, name any feature flags and note the rollout plan. Skip when the change is a simple fix.]
```

## Guidelines

- Use domain terms from `CONTEXT.md` exactly — do not paraphrase
- Reference ADR numbers for decisions already recorded in `docs/adrs/`
- User stories must cover all user types affected by the change
- Implementation Decisions name modules by domain — not file paths
- Every acceptance criterion must map to at least one user story
- Every `Assumed:` line in Open Questions is a flag for the human to confirm
- If you skipped questions because the input was complete, say so at the top: *"Generated without questions — input was sufficient. Verify Assumed: items below."*
- Success Metrics and Rollout are conditional — include when the problem warrants it, not for every PRD
