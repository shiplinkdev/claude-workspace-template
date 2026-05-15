# Git Branching & Commit Rules

## Branch Names

Pattern: `<type>/<issue-number>/<short-description>`

- **Types:** `feat`, `fix`, `refactor`, `ci`, `docs`, `chore`, `build`, `test`
- **Issue number:** always include the Linear/GitHub issue number when one exists
- **Multiple issues:** join with a hyphen — `feat/1988-1991/short-description`
- **No issue number:** only for release, chore, or purely infra branches
- Use lowercase kebab-case for the description segment

Examples:
```
feat/796/copy-clipboard-action
fix/791/start-navigation-logic
fix/1312-1310/anonchat-fileextension-fixes
ci/428/newrelic-implementation
release/20260428-1
```

## Commit Messages

Pattern: `<type>(<issue-number>): <short description>`

- **Issue number in subject:** include as the scope when branch has an issue number
- **Commit body:** always add `Refs #<issue-number>` on a separate line when there is an issue number
- **Multiple issues:** list each ref on its own line — `Refs #1312`, `Refs #1310`
- Keep the subject line under 72 characters; use imperative mood

Examples:
```
feat(796): add copy to clipboard action

Refs #796
```
```
fix(791): update start navigation logic

Refs #791
```
```
refactor: remove unused requests
```
(no Refs line when there is no issue number)

## Committing

- Before staging, always run `git diff` on all modified files to understand every change
- Never stage files selectively without first reviewing what other unstaged changes exist
- After every commit, run `git status` to confirm the working tree is clean — if it isn't, determine whether the remaining changes belong to the same task and commit them too

## PR Workflow

### Feature / fix branches (`feat/*`, `fix/*`, `refactor/*`, etc.)

1. Open PR → **`develop`** (never directly to `main`)
2. Once the PR is reviewed and passes, create a release branch from the working branch:
   - Pattern: `release/YYYYMMDD-<count>` — e.g. `release/20260505-1`, `release/20260505-2`
   - Count starts at 1 and increments if multiple releases go out on the same day
3. Open PR from the release branch → **`main`**

### Release branches (`release/*`)

- Branch off the feature/fix branch after it is approved on `develop`
- PR target: **`main`**
- No further feature work goes on a release branch
