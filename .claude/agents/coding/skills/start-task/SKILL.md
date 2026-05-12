---
name: start-task
description: Given a parent GitHub issue number, check the ready-for-agent label, pick the next unblocked subtask, implement it following your team's conventions, and open a PR targeting the integration branch. Stops after one subtask.
---

# start-task

Given a parent issue number, find the next unblocked subtask, implement it, and open a PR. Stop after one subtask.

## Process

1. Check the parent issue has the `ready-for-agent` label — if not, stop and tell the user
2. Fetch all open subtasks of the parent issue
3. Pick the next unblocked subtask: lowest-numbered issue where all "Blocked by" issues are closed
4. Read the subtask title, body, and **all comments** — this is your primary spec
5. If the description is unclear, post a comment on the issue with your interpretation before writing code
6. Check whether the task provides a working branch — use it if yes; otherwise create one following your git rules
7. Implement the task — minimal, targeted changes only
8. Run the build and fix any errors
9. Commit following your team's commit format
10. Push and open a PR targeting the integration branch
11. Stop — one subtask per session

<!-- TODO: Customize this skill's process to match your team's workflow.
     For example:
     - Replace "run the build" with your actual verification command (e.g. `npm run build`, `cargo build`)
     - Replace "integration branch" with your actual branch name (e.g. `develop`, `main`, `staging`)
     - Add any repo-specific steps (e.g. running migrations, updating snapshots) -->
