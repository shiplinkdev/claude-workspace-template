# to-tasks: PRD → GitHub Sub-Issues

## Purpose

Break a PRD into independently-implementable vertical slices and create them as GitHub sub-issues under a parent issue.

**Input**: PRD (pasted by user, or fetched from a GitHub issue number)
**Output**: GitHub sub-issues created via `gh` CLI

## Trigger

User invokes `/to-tasks #<issue>` or `/to-tasks` with a PRD pasted.

## Process

### 1. Gather Context

If an issue number is given:
```bash
gh issue view <number> --repo <org>/<repo>
```

Read the issue body as the PRD source. If the user pastes a PRD directly, use that.

### 2. Read Codebase Context

- `CONTEXT.md` — domain terms and existing decisions
- `docs/adrs/` — respect existing architectural decisions
- Explore relevant directories to understand current state of affected modules

### 3. Draft Vertical Slices

Each slice must be:
- **Thin but complete** — touches all layers it needs for its slice of functionality
- **Independently demoable** — produces something verifiable without depending on other slices being done
- **Ordered** — dependencies resolved first

Slices are NOT horizontal layers (e.g., "build all the APIs first"). Each slice delivers a narrow but complete user-facing behavior.

Classify each slice:
- **AFK** — can be implemented and merged without human interaction (preferred)
- **HITL** — requires human decision (architectural, design review, external dependency)

### 4. Present Slices for Review

Show the user the slice list with:
- Title
- Type (AFK / HITL)
- Blocking relationships
- Which acceptance criteria from the PRD it covers

Ask: "Does this breakdown look right? Any slices to split, merge, or reorder?"

Wait for approval before creating issues.

### 5. Create GitHub Sub-Issues

Once approved, create in dependency order:

```bash
gh issue create \
  --repo <org>/<repo> \
  --title "<slice title>" \
  --body "<issue body>"
```

For each created issue, add it as a sub-issue of the parent via the GitHub API:
```bash
gh api --method POST /repos/<org>/<repo>/issues/<parent>/sub_issues \
  --field sub_issue_id=<new-issue-id>
```

After all subtasks are created, print a reminder:

```
All subtasks created. Add the `ready-for-agent` label to the parent issue (#<parent>) when you're ready for the Coding Agent to begin work:

  gh issue edit <parent> --repo <org>/<repo> --add-label "ready-for-agent"
```

Do not add the label automatically — it is a deliberate human signal to start agent work.

### Issue Body Template

```markdown
## Summary
[What this slice delivers from the user's perspective]

## Acceptance Criteria
- [ ] [Specific, testable]

## Parent PRD
Refs #<parent-issue>

## Blocked by
- #<issue> (if applicable)

## Notes
[Domain constraints, ADR references, implementation hints — no file paths]
```

## Constraints

- Use domain terms from `CONTEXT.md` exactly
- Reference ADR numbers where architectural decisions apply
- No file paths in issue bodies — reference modules by domain name
- Do not create issues for "open questions" — those must be resolved in the PRD first
