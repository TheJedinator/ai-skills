---
name: stacked-prs
description: Build and restructure a stack of PRs with `gh stack` while the work is in progress — create the chain, add a layer, fix a lower branch and restack, sync after main moves, split, reorder or fold PRs without closing them. Use whenever working with dependent/stacked PRs before they land, or on "stack this", "split this PR", "rebase the stack", "restack", "cascade rebase", "merge main down" or "restructure the stack". To land a finished stack, use `babysit-pr-stack`.
---

# Stacked PRs

A stack is an ordered chain of branches. Each branch builds on the one below it, and each branch has one PR. The bottom PR targets the repo's default branch (the trunk). Each higher PR targets the branch below it, so reviewers see only that layer.

```
<user>/<ticket>-3-endpoint → PR #3 (base: <ticket>-2-service)  ← top
<user>/<ticket>-2-service  → PR #2 (base: <ticket>-1-models)
<user>/<ticket>-1-models   → PR #1 (base: main or master)      ← bottom
```

This skill covers the stack while you build it. To merge it, use `babysit-pr-stack`.

## Rules

- Use `gh stack` for every stack operation. Install it once with `gh extension install github/gh-stack`.
- Find the trunk; do not assume it: `gh repo view --json defaultBranchRef -q .defaultBranchRef.name`. It is `main` or `master`, depending on the repo.
- Never close and recreate PRs to restructure a stack. Closing loses review threads and approvals. The steps below change the stack and keep every PR.
- Open PRs ready for review, not as drafts.
- Keep each PR reviewable. A large mechanical change (a rename, a move) goes in its own PR at the bottom of the stack.
- Number branches so the stack order is visible: `<user>/<ticket>-<n>-<slug>`.

## Create a stack

```bash
gh stack init <user>/<ticket>-1-models       # creates and checks out the bottom branch on the trunk
# ... commit ...
gh stack add <user>/<ticket>-2-service       # creates the next branch on top of the current one
# ... commit ...
gh stack push                                # pushes every branch, each with --force-with-lease
```

Open each PR with the `creating-pull-requests` skill, bottom first. Pass `--base` as the branch below; the bottom PR's base is the trunk. Then link the PRs into a stack on GitHub:

```bash
gh stack sync
```

`gh stack submit` also opens PRs, but it is an interactive editor. `gh stack submit --auto` makes draft PRs with generated titles. Do not use either one as an agent.

To adopt branches that already exist, list them bottom to top: `gh stack init <branch-1> <branch-2> ...`.

## Fix a lower branch

Review feedback or a bug in a lower PR: commit the fix on the branch that owns the code, then restack the branches above it.

```bash
gh stack checkout <lower-branch>             # or: gh stack down [n]
# ... commit ...
gh stack rebase --upstack                    # rebase this branch's descendants onto it
gh stack push
```

After review starts, add the fix as a new commit. Do not amend or autosquash it into an earlier commit.

## Sync with the trunk

After review starts, rebase onto the trunk only for a conflict or a restack. Each push re-runs CI on every pushed PR.

```bash
gh stack sync                                # fetch, rebase the stack if the trunk moved, push, refresh PR state
```

If the rebase conflicts, `sync` restores every branch and stops. Then run `gh stack rebase`, resolve each conflict, `git add` the files, and run `gh stack rebase --continue`. To give up, run `gh stack rebase --abort`.

When a lower PR merges with a squash, `gh stack rebase` switches to `--onto` mode, so the old commits do not conflict with the squash commit. Do not run `git rebase <trunk>` by hand on a stack: after a squash merge it replays the merged commits.

## Restructure a stack

`gh stack modify` drops, folds, inserts, reorders and renames branches, then `gh stack submit` pushes the result. It keeps the PRs. It is an interactive terminal UI, so an agent cannot run it. Ask the user to run it for a reorder or fold.

An agent can split a PR without `modify`:

1. Create a branch at the last commit that belongs in the lower part: `git branch <user>/<ticket>-2a-<slug> <sha>`.
2. Unlink the stack on GitHub and locally: `gh stack unstack`. The PRs stay open, with their reviews.
3. Track the branches again, bottom to top: `gh stack init <branch-1> <branch-2a> <branch-2> ...`.
4. Push, then open a PR for the new branch with the `creating-pull-requests` skill.
5. Point every PR at the branch below it: `gh pr edit <pr> --base <branch-below>`.
6. Link the stack again: `gh stack sync`.

A renamed branch on GitHub gets a new PR, because a PR's head branch cannot change. Rename a branch only before its PR is open.

## Check the stack

After any rebase or restructure:

```bash
gh stack view --short                        # order, branches, PR numbers
gh pr list --author "@me" --state open --json number,headRefName,baseRefName \
  --jq '.[] | "\(.number): \(.headRefName) → \(.baseRefName)"'
```

Every PR's base must be the branch below it, and the bottom PR's base must be the trunk. Then run the repo's lint and tests on the top branch. If something is wrong, fix it and check again.
