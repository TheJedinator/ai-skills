---
name: creating-pull-requests
description: Creates a GitHub pull request with a "## Why" / "## What" body written from the branch diff. Works in any repo and any language. Use when the user asks to create a PR, open a pull request, or make a PR.
---

# Creating Pull Requests

Write a PR description from the branch diff, then open the PR with `gh pr create`.

## Steps

### 1. Run the repo's checks

If the repo documents pre-PR checks (CONTRIBUTING, README, Makefile, CI config), run them. Fix failures before you continue. If it documents none, skip this step.

### 2. Write the description

1. Find the default branch: `gh repo view --json defaultBranchRef -q .defaultBranchRef.name`
2. Read the diff: `git diff <default-branch>...HEAD` (three dots: changes since this branch diverged).
3. Write for a developer who is about to review it. Keep it quick to read.

If the repo has a PR template (`.github/pull_request_template.md`), follow it. Otherwise use:

```markdown
## Why

[The problem or motivation. Why this change was needed.]

## What

[The changes made to address the Why.]

[Special notes if applicable: deployment steps, breaking changes.]
```

### 3. Push and create

```bash
git push -u origin <branch-name>

gh pr create \
  --title "<short title>" \
  --body "$(cat <<'EOF'
<description>
EOF
)"
```

Add `--assignee @me` or `--reviewer <user-or-team>` only when the repo has a reviewing team or the user asks. Add `--base <branch>` when the PR targets a branch other than the default, such as a stacked PR.

## Rules

- Push before `gh pr create`.
- Use the heredoc form for `--body`. Inline text breaks on newlines and quotes.
- Use three dots (`main...HEAD`), not two (`main..HEAD`).
