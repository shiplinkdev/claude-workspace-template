---
name: tdd
description: Run a red-green-refactor loop for a single subtask using E2E tests. Use when the subtask has clear acceptance criteria suitable for browser-level or integration testing. Skip for pure server-side logic or tasks with no observable behavior.
---

# tdd

Red-green-refactor loop for a single task using your team's E2E test framework.

## When to use

Use when the subtask has clear acceptance criteria that can be verified at the UI or API boundary. Skip for:
- Pure server-side logic with no visible output
- Data transformation / adapter layers
- Tasks with no testable external behavior

## Process

1. **Red** — write a failing test that captures the acceptance criterion
2. **Green** — write the minimum code to make the test pass
3. **Refactor** — clean up without breaking the test

Run the test suite at each phase. Verify no regressions in existing tests.

<!-- TODO: Customize this skill to match your test runner and framework.
     For example:
     - Replace with your actual test command (e.g. `npx playwright test`, `jest`, `cypress run`)
     - Specify which test directory new tests should live in
     - Add any environment setup steps needed before tests can run
     - Note any patterns for how test files are named in your project -->
