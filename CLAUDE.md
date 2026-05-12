# [Your Project Name] — Claude Code

## Workflow

<!-- TODO: Describe any setup steps Claude should run before starting work.
     Example from a Next.js project:
       Always fetch the latest API schema first:
       - URL: https://your-api.example.com/openapi.json
       - Save to: openspec/contracts/api-schema.json -->

### API Contract

<!-- TODO: If your project has an API schema or contract file, point to it here.
     Example: `openspec/contracts/api-schema.json` — always treat as source of truth -->

---

@.claude/rules/git.md
@.claude/rules/structure.md <!-- TODO: create this file -->
@.claude/rules/stack.md <!-- TODO: fill in this file -->

---

## Read When Relevant

Load the rule file when your task touches that area. Do not load all of them upfront.

| When you're working on…         | Read |
|---------------------------------|------|
| Components, naming, annotations | `.claude/rules/components.md` |
| API route handlers              | `.claude/rules/api.md` |
| Adapters / data transforms      | `.claude/rules/adapters.md` |
| Service layer / business logic  | `.claude/rules/services.md` |
| State management                | `.claude/rules/store.md` <!-- TODO: rename/replace to match your state layer --> |
| Constants naming & location     | `.claude/rules/constants.md` |
| Utility / helper functions      | `.claude/rules/utils.md` <!-- TODO: rename/replace to match your project --> |
| Styling / CSS                   | `.claude/rules/styling.md` |
| Forms & validation              | `.claude/rules/forms.md` |
| Import order, formatter, types  | `.claude/rules/code-style.md` |
| Tests                           | `.claude/rules/testing.md` |
| Domain concepts & calculations  | `.claude/rules/domain.md` <!-- TODO: create if your project has domain rules --> |

---

## Skills

| Skill | What it does |
|-------|-------------|
| `/spike` | Time-boxed investigation when you're uncertain — explores options, surfaces unknowns, recommends next step |
| `/grill` | Design interview → PM Handoff Summary (reads CONTEXT.md + ADRs) |
| `/to-tasks #<issue>` | PRD → GitHub sub-issues via `gh` CLI |
| `/caveman` | Ultra-compressed mode (~75% fewer tokens) |
| `/to-prd` | PM Agent only — Handoff Summary → full PRD |
| `/problem-to-prd` | Raw problem/bug/feature → full PRD (skips grill — focused-ask, max 3 questions) |
| `/start-task #<issue>` | Pick next unblocked subtask from a parent issue, implement it, and open a PR |

### Context & Decisions

- `CONTEXT.md` — living domain glossary; update during grill sessions when terms resolve
- `docs/adrs/` — Architecture Decision Records; create conservatively (hard-to-reverse, non-obvious decisions only)

---

## Platform Agents (platform.claude.com)

System prompts and skill definitions live in `.claude/agents/` — version-controlled source of truth.

| Agent | System prompt | Skills |
|-------|---------------|--------|
| PM Agent | `.claude/agents/pm/system-prompt.md` | `to-prd.md`, `to-task.md`, `problem-to-prd.md` |
| Coding Agent | `.claude/agents/coding/system-prompt.md` | `tdd.md`, `start-task.md` |
| Triage Agent | `.claude/agents/triage/system-prompt.md` | — |

---

## Further Reading

<!-- TODO: Add links to deeper technical docs as your project grows.
     Example:
     | File | When to use |
     |------|-------------|
     | `docs/technical/stack.md`       | Full tech stack details |
     | `docs/architecture/api.md`      | API architecture deep-dive |
     | `docs/development/practices.md` | General dev practices | -->
