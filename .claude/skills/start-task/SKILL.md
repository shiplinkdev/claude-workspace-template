# start-task

## Purpose

Given a parent issue number, find the next unblocked subtask, implement it, and open a PR. Stop after one subtask.

## Input

A GitHub parent issue number — the feature-level issue that owns the subtask breakdown.

## Process

### 1. Check the parent issue

```bash
gh issue view <parent-number> --repo TODO:your-org/your-repo --json title,body,labels
```

If the parent does **not** have the `ready-for-agent` label, stop and tell the user: "This issue is not labeled `ready-for-agent`. Add the label when you're ready for agent work to begin."

### 2. Fetch open subtasks

```bash
gh api /repos/TODO:your-org/your-repo/issues/<parent-number>/sub_issues \
  --jq '[.[] | {number: .number, title: .title, state: .state}]'
```

Cross-reference the parent's sub-issues list to get only this feature's subtasks.

### 3. Pick the next subtask

Select the first open subtask where all issues listed in its "Blocked by" section are closed. If multiple subtasks are unblocked, pick the lowest-numbered one.

If no unblocked subtasks remain, report: "All subtasks are either complete or blocked. No work to pick up."

### 4. Read the task spec

The subtask title, body, and **all comments** are your primary spec. Fetch with:

```bash
gh issue view <number> --repo TODO:your-org/your-repo --json title,body,labels
```

Read:
- **Title** — what this slice delivers
- **Acceptance Criteria** — what done looks like
- **Notes** — domain constraints and hints
- **Blocked by** — already checked in step 3

If the task description is unclear, post a comment on the issue summarizing your interpretation before writing any code:

```bash
gh issue comment <number> --repo TODO:your-org/your-repo \
  --body "Starting work. My interpretation: <summary>. Proceeding unless corrected."
```

If the subtask references the parent PRD for broader context, fetch it:
```bash
gh issue view <parent-number> --repo TODO:your-org/your-repo
```

### 5. Read required rules

Always:
- `CLAUDE.md`
- `.claude/rules/git.md`
- `.claude/rules/structure.md`
- `.claude/rules/stack.md`

Then load the rule files relevant to this task (see the table in `CLAUDE.md`).

### 6. Explore affected code

Search for existing modules, services, and patterns this task will touch. Do not duplicate — extend or modify existing code.

### 7. Branch

Check whether the task provides a working branch. If yes, check it out:

```bash
git checkout <provided-branch>
```

If no branch is provided (or the provided branch is unusable), create one following `.claude/rules/git.md` naming:

```
<type>/<issue-number>/<short-description>
```

```bash
git checkout -b <branch-name>
```

### 8. Implement

Write only what the subtask requires. Follow all rules loaded in step 5.

### 9. Verify

Before committing, run your project's build/verify command:

```bash
# TODO: replace with your project's build or lint command
# e.g. yarn build / npm run build / go build ./... / flutter build
```

Fix any errors or build failures before proceeding.

### 10. Commit

Follow `.claude/rules/git.md` commit format exactly.

```
<type>(<issue-number>): <short description>

Refs #<issue-number>
```

### 11. Push and open PR

```bash
git push -u origin <branch-name>

gh pr create \
  --repo TODO:your-org/your-repo \
  --title "<type>(<issue-number>): <subtask title>" \
  --base develop \
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

### 12. Stop

Report the PR URL. Do not pick up the next subtask — wait for the PR to be reviewed before continuing.
