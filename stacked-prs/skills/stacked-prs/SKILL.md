---
name: stacked-prs
description: Manage stacked pull requests — creating PR chains, cascade rebasing after changes, merging master down after a parent ships, restructuring/splitting stacks, and the nuclear "close all and recreate" option. Use whenever working with dependent/stacked PRs, when a parent PR merges and children need updating, when PRs need to be split or reordered, or when branch names have drifted from PR content. Also triggers on "merge master down", "rebase the stack", "cascade rebase", "split this PR", or "restructure the stack".
---

# Stacked PRs

Stacked PRs are a chain of pull requests where each builds on the previous one. They enable incremental review of large features while keeping each PR small and focused.

## Mental Model

```
master
  └── PR 1 (branch: feature-1-models)      ← targets master
       └── PR 2 (branch: feature-2-service) ← targets feature-1-models
            └── PR 3 (branch: feature-3-endpoint) ← targets feature-2-service
```

Each PR's `--base` is the branch of its parent PR. When PR 1 merges to master, PR 2 rebases onto master and retargets `--base master`.

## Creating a Stack

Build each branch from its parent. Push and create PRs with explicit `--base`:

```bash
# PR 1
git checkout -b user/feature-1-models master
# ... implement ...
git push -u origin user/feature-1-models
gh pr create --base master --title "Part 1: Models" --assignee @me

# PR 2
git checkout -b user/feature-2-service user/feature-1-models
# ... implement ...
git push -u origin user/feature-2-service
gh pr create --base user/feature-1-models --title "Part 2: Service" --assignee @me

# PR 3
git checkout -b user/feature-3-endpoint user/feature-2-service
# ... implement ...
git push -u origin user/feature-3-endpoint
gh pr create --base user/feature-2-service --title "Part 3: Endpoint" --assignee @me
```

### Naming Convention

Number branches to make stack order obvious:

```
user/ticket-1-first-change
user/ticket-2-second-change
user/ticket-3-third-change
```

This prevents confusion when restructuring — branch names immediately tell you the order.

## Cascade Rebase (Parent Changed)

When you amend or add commits to a parent branch, all children must rebase. Work top-down from the changed branch:

```bash
# Parent (PR 1) was amended
git checkout user/feature-2-service
git rebase user/feature-1-models
git push --force-with-lease

git checkout user/feature-3-endpoint
git rebase user/feature-2-service
git push --force-with-lease
```

### Handling "already applied" conflicts

After restructuring, git may try to replay commits that are already in the parent. You'll see:

```
Could not apply abc123... Some commit message
```

If the commit is already part of the new base (common after amending a parent), skip it:

```bash
git rebase --skip
```

Only skip when you're certain the commit's changes are already upstream. If unsure, check with `git log --oneline <base-branch>` to confirm the work is there.

### GIT_EDITOR trick for non-interactive continue

When all conflicts are resolved but git wants you to edit the commit message:

```bash
GIT_EDITOR=true git rebase --continue
```

## Merging Master Down (Parent PR Shipped)

When a parent PR merges to master, the next child becomes the new stack root:

```bash
# PR 1 just merged to master
git checkout master && git pull

# Rebase PR 2 onto master (PR 1's commit will be skipped automatically)
git checkout user/feature-2-service
git rebase master
git push --force-with-lease

# Update PR 2's base to master
gh pr edit <pr-number> --base master

# Cascade to remaining children
git checkout user/feature-3-endpoint
git rebase user/feature-2-service
git push --force-with-lease
```

The key insight: `git rebase master` on a child branch will automatically skip commits that are now in master (they were part of the merged parent PR). You may still need `git rebase --skip` for edge cases.

## Restructuring a Stack

When PRs need to be split, reordered, or renamed, the cleanest approach is the nuclear option: close all PRs, create fresh branches, open new PRs. Trying to rename branches or change targets on open stacked PRs leads to chaos.

### The Nuclear Option

```bash
# 1. Close all PRs in the stack
gh pr close <pr-1-number>
gh pr close <pr-2-number>
gh pr close <pr-3-number>

# 2. Delete old remote branches
git push origin --delete old-branch-1 old-branch-2 old-branch-3

# 3. Create new branches pointing at the correct commits
git branch user/feature-1-new-name <commit-sha-for-pr1>
git branch user/feature-2-new-name <commit-sha-for-pr2>
git branch user/feature-3-new-name <commit-sha-for-pr3>

# 4. Push all new branches
git push origin user/feature-1-new-name user/feature-2-new-name user/feature-3-new-name

# 5. Create new PRs with correct stacking
gh pr create --head user/feature-1-new-name --base master --title "..." --assignee @me
gh pr create --head user/feature-2-new-name --base user/feature-1-new-name --title "..." --assignee @me
gh pr create --head user/feature-3-new-name --base user/feature-2-new-name --title "..." --assignee @me

# 6. Clean up old local branches
git branch -D old-branch-1 old-branch-2 old-branch-3
```

### When to go nuclear

- Branch names no longer match PR content (from splitting/reordering)
- GitHub closed a PR due to force-push divergence (can't reopen)
- The stack order needs to change
- PRs need to be split into more granular pieces

### Splitting a PR

If a PR does too much, extract the commits:

```bash
# Start a new branch from the parent
git checkout -b user/feature-2a-first-part user/feature-1-parent

# Cherry-pick only the relevant commits (or implement fresh)
# ... make changes for just part A ...
git commit -m "Add first part"

# Now create the second branch from the first
git checkout -b user/feature-2b-second-part user/feature-2a-first-part
# ... make changes for just part B ...
git commit -m "Add second part"
```

## Verification After Any Stack Operation

After any rebase or restructure, verify the stack is healthy:

```bash
# Check all PRs are open with correct bases
gh pr list --author "@me" --state open \
  --json number,title,headRefName,baseRefName \
  --jq '.[] | select(.headRefName | contains("feature")) | "\(.number): \(.title)\n  \(.headRefName) → \(.baseRefName)\n"'

# Run tests on the leaf branch (catches everything in the stack)
git checkout <leaf-branch>
# Run your project's lint, typecheck, and test commands here
```

## Common Pitfalls

| Pitfall | Solution |
|---------|----------|
| GitHub auto-closes PR on force-push | Go nuclear — close all, recreate with fresh branches |
| `git rebase` replays already-merged commits | `git rebase --skip` when the commit is in the new base |
| Branch name doesn't match PR content | Go nuclear — rename costs less than confusion |
| Forgetting `--force-with-lease` | Always use it (not `--force`) — it protects against overwriting others' pushes |
| Rebasing in wrong order (child before parent) | Always rebase top-down: parent first, then children in order |
| `gh pr edit --base` without rebasing first | Always rebase onto new base BEFORE updating the PR target |
| Closing an unrelated PR when batch-closing | Filter by branch name pattern, not blanket close |
