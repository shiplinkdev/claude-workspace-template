# claude-workspace-template

A GitHub template repository containing the stack-agnostic layer of a Claude Code workflow. Fork it, add your stack-specific rules, and your team has a working Claude Code setup in minutes.

---

## What this is

This template gives your team:

- **7 workflow skills** — `/grill`, `/spike`, `/to-prd`, `/to-tasks`, `/problem-to-prd`, `/caveman`, `/start-task`
- **3 platform agent skeletons** — PM Agent, Coding Agent, Triage Agent (for platform.claude.com)
- **Git conventions** — branching, commit format, WSL workaround (`.claude/rules/git.md`)
- **10 rule file stubs** — one per category, each with a TODO comment explaining what to write
- **CLAUDE.md skeleton** — progressive disclosure pattern, skills table, agent table
- **CONTEXT.md skeleton** — domain glossary, roles, constraints, ADR index

What it does **not** include: stack-specific rules, domain knowledge, or product context. Those are yours to write.

---

## Prerequisites

- [Claude Code CLI](https://docs.anthropic.com/en/docs/claude-code) installed
- A GitHub account with access to your org's repos
- `gh` CLI installed and authenticated (`gh auth login`)

---

## Getting started

### 1. Fork the template

Click **Use this template → Create a new repository** on GitHub. Choose your org, name the repo (e.g. `yourproject-claude-workspace`), and create it.

### 2. Clone and open with Claude Code

```bash
git clone https://github.com/your-org/your-repo.git
cd your-repo
claude .
```

### 3. Follow the setup checklist below

---

## What to keep

These files are ready to use as-is:

| File / Directory | Why it's ready |
|------------------|----------------|
| `.claude/skills/` | All 7 skills work out of the box — generic workflow logic, no project-specific content |
| `.claude/rules/git.md` | Branching conventions and WSL commit workaround apply to any project |
| `.claude/agents/*/system-prompt.md` | Agent role definitions and workflow logic are generic — just fill in the TODO blocks |
| `CLAUDE.md` (structure) | The progressive disclosure pattern and table structure are correct — replace placeholder values |
| `CONTEXT.md` (structure) | Section headers and purpose comments are correct — fill in your domain content |
| `docs/adrs/README.md` | Explains the ADR convention — no changes needed |

---

## What to replace

Work through these in order:

### 1. Fill in the rule stubs (`.claude/rules/`)

Each stub file contains a single TODO comment explaining what to write. Open each one and fill it in for your stack:

| File | Write about |
|------|-------------|
| `stack.md` | Runtime, framework, key libraries, build tools, test runner |
| `structure.md` | *(create this file)* — directory layout, where each file type lives |
| `components.md` | Component types, naming, co-location, annotation requirements |
| `api.md` | Route handler pattern, auth, request/response conventions |
| `adapters.md` | Adapter layer purpose, naming, data flow |
| `services.md` | Service layer responsibilities, organization, error handling |
| `forms.md` | Form library, validation, schema location, server error handling |
| `styling.md` | CSS strategy, design tokens, breakpoints |
| `testing.md` | Test framework, file location, how to run, required credentials |
| `constants.md` | Where constants live, naming convention |
| `code-style.md` | Import order, formatter config, linter rules |

### 2. Update `CLAUDE.md`

Replace every `TODO` placeholder:

- Project name in the heading
- API schema URL and path (if your project has one)
- `@` imports — update or remove rule file references that don't apply to your stack
- Read When Relevant table — remove rows for rule files you don't use; add rows for any you add
- Further Reading section — link to your deeper docs as they exist

### 3. Fill in `CONTEXT.md`

- **Domain Glossary** — add your core domain terms with one-line definitions
- **Entity Lifecycle** — map out the states and transitions of your key entities
- **Roles & Permissions** — list your user roles and what each can do
- **Key Constraints** — hard architectural or business constraints that affect all decisions

### 4. Update `start-task` repo references

In `.claude/skills/start-task/SKILL.md`, replace every `TODO:your-org/your-repo` with your actual org and repo name:

```bash
# Find all occurrences
grep -n "TODO:your-org/your-repo" .claude/skills/start-task/SKILL.md
```

Also replace the build/verify TODO comment with your actual command (e.g. `yarn build`, `npm run build`, `go build ./...`).

---

## Configuring platform agents (platform.claude.com)

The PM Agent, Coding Agent, and Triage Agent skeletons are in `.claude/agents/`. Each has a `system-prompt.md` you copy into platform.claude.com.

### Steps for each agent

1. **Open** `.claude/agents/<agent>/system-prompt.md` in your editor
2. **Fill in every TODO block** — embed your stack rules, domain glossary, and product description directly in the prompt. Platform agents cannot read local files, so everything must be self-contained.
3. **Copy** all content below the `---` separator
4. **Go to** platform.claude.com → your org → Agents → create or edit the agent
5. **Paste** into the system prompt field
6. **Add skills** — for the Coding Agent, attach `start-task` and `tdd` from your org's skill library; for the PM Agent, attach `to-prd`, `to-task`, and `problem-to-prd`
7. **Publish**

Keep `.claude/agents/*/system-prompt.md` as your source of truth. When you update an agent, edit the file first and re-paste — don't edit directly on platform.claude.com.

> **Why self-contained?** Platform agents run in the cloud and have no access to your local repo files unless an MCP server is explicitly attached. All rules must be embedded in the prompt itself. See `docs/adrs/` for the original decision record on this.

---

## Architecture Decision Records

ADRs live in `docs/adrs/`. See [`docs/adrs/README.md`](docs/adrs/README.md) for what they are and when to create one.

The short version: create an ADR when you make a hard-to-reverse architectural decision that future readers wouldn't infer from the code alone.

---

## Skills reference

| Skill | Trigger | What it does |
|-------|---------|-------------|
| `/spike` | Uncertainty about approach | Time-boxed investigation — explores options, surfaces unknowns |
| `/grill` | Feature idea or GitHub issue | Design interview → PM Handoff Summary |
| `/to-prd` | PM Handoff Summary | Converts handoff into a full PRD (PM Agent) |
| `/to-tasks #<issue>` | Finalized PRD | Breaks PRD into GitHub sub-issues |
| `/problem-to-prd` | Bug report or feature request | Raw input → PRD, max 3 clarifying questions |
| `/caveman` | Long session, token-heavy | Ultra-compressed responses (~75% fewer tokens) |
| `/start-task #<issue>` | Parent issue number | Picks next unblocked subtask, implements it, opens PR |
