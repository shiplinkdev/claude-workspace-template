---
name: start-feature-task
description: Given a parent GitHub issue number, check the ready-for-agent label, pick the next unblocked subtask, implement it on a branch off the parent feature branch, and open a PR targeting that parent branch (not develop). Use this instead of start-task when the feature has multiple dependent subtasks that must land together — subtasks stack on a shared parent branch, and develop gets one PR when all subtasks are merged. Stops after one subtask.
---

# Coding Agent Skill: start-feature-task

## Purpose

Given a parent issue number, stack subtask work on a shared parent feature branch. Each subtask PRs into the parent branch — `develop` gets a single PR only after all subtasks are merged.

Use this instead of `start-task` when:
- Subtasks share foundational work (design tokens, shared components) that must exist before later tasks can build on it
- The feature should land as one unit in `develop`, not as a stream of unrelated PRs

## Input

A GitHub parent issue number — the feature-level issue that owns the subtask breakdown.

## Process

### 1. Check the parent issue

```bash
gh issue view <parent-number> --repo shiplinkdev/shiplink-frontend-ui --json title,body,labels,subIssues
```

If the parent does **not** have the `ready-for-agent` label, stop and tell the user: "This issue is not labeled `ready-for-agent`. Add the label when you're ready for agent work to begin."

### 2. Ensure the parent feature branch exists

Derive the parent branch name from the issue title using `.claude/rules/git.md` naming:

```
feat/<parent-number>/<kebab-case-title>
```

Example: issue #842 "Website New Design Migration" → `feat/842/website-new-design-migration`

Check whether it already exists:

```bash
git fetch origin
git branch -a | grep feat/<parent-number>/
```

**If the parent branch does not exist** — create it from an up-to-date `main`:

```bash
git checkout main
git pull origin main
git checkout -b feat/<parent-number>/<short-description>
git push -u origin feat/<parent-number>/<short-description>
```

**If it already exists** — check it out and pull latest:

```bash
git checkout feat/<parent-number>/<short-description>
git pull origin feat/<parent-number>/<short-description>
```

### 3. Fetch open subtasks

```bash
gh issue list --repo shiplinkdev/shiplink-frontend-ui \
  --search "is:open" \
  --json number,title,body,labels,state
```

Cross-reference with the parent's `subIssues` list to get only this feature's subtasks.

### 4. Pick the next subtask

Select the first open subtask where all issues listed in its "Blocked by" section are **closed**. If multiple subtasks are unblocked, pick the lowest-numbered one.

**If no unblocked subtasks remain:**
- All subtasks closed → tell the user: "All subtasks are complete. The parent branch `feat/<parent-number>/...` is ready. Open a PR from it to `develop`, then follow the release procedure."
- Some open but all blocked → report the blocking chain.

### 5. Read the task spec

```bash
gh issue view <subtask-number> --repo shiplinkdev/shiplink-frontend-ui --comments
```

Read: Title, Acceptance Criteria, Comments (scope changes, decisions), Notes, Blocked by.

If the task references the parent PRD for broader context:
```bash
gh issue view <parent-number> --repo shiplinkdev/shiplink-frontend-ui
```

If the description is unclear after reading all comments, post your interpretation before writing code:

```bash
gh issue comment <subtask-number> --repo shiplinkdev/shiplink-frontend-ui \
  --body "Starting work. My interpretation: <summary>. Proceeding unless corrected."
```

### 6. Read required rules

Always:
- `CLAUDE.md`
- `.claude/rules/git.md`
- `.claude/rules/structure.md`
- `.claude/rules/stack.md`

Then load rule files relevant to this task (see the table in the system prompt).

### 7. Explore affected code

Search for existing modules, services, adapters, and store slices this task will touch. Do not duplicate — extend or modify existing code.

### 8. Branch off the parent feature branch

Make sure you're on the parent branch (step 2 ensures this), then create the subtask branch:

```bash
git checkout feat/<parent-number>/<short-description>
git checkout -b feat/<subtask-number>/<short-description>
```

Naming: `feat/<subtask-number>/<kebab-case-description>`

### 9. Implement

Write only what the subtask requires. Follow all rules loaded in step 6.

> **TDD bypassed for now** — do not run `/tdd` or write Playwright tests. Implement directly.

### 10. Verify

```bash
yarn build
```

Fix any type errors or build failures before committing.

### 11. Commit

Follow `.claude/rules/git.md` commit format. In WSL, use the wsl commit pattern to avoid hook failures.

```
feat(<subtask-number>): <short description>

Refs #<subtask-number>
Refs #<parent-number>
```

### 12. Push and open PR targeting the parent branch

```bash
git push -u origin feat/<subtask-number>/<short-description>

gh pr create \
  --repo shiplinkdev/shiplink-frontend-ui \
  --title "feat(<subtask-number>): <subtask title>" \
  --base feat/<parent-number>/<short-description> \
  --body "$(cat <<'EOF'
## Summary
<what this slice delivers>

## Acceptance Criteria
- [ ] <from the issue>

## References
Closes #<subtask-number>
Refs #<parent-number>
EOF
)"
```

### 13. Stop

Report the PR URL. Then tell the user:

> "PR opened targeting `feat/<parent-number>/...`. Merge it, then run `/start-feature-task <parent-number>` again to pick the next subtask."

Do not pick up the next subtask — one subtask per invocation.

---

## When all subtasks are done

Once every subtask is closed and merged into the parent branch, the user should:

1. **Open PR: parent branch → `develop`**
2. **After develop merge is approved — cut a release branch from the parent branch:**
   ```bash
   git checkout feat/<parent-number>/<short-description>
   git pull
   git checkout -b release/YYYYMMDD-N
   git push -u origin release/YYYYMMDD-N
   ```
3. **Open PR: release branch → `main`**
