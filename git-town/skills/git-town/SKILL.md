---
name: git-town
description: Use when working with feature branches in repos with git town installed - creating branches, syncing with main, creating stacked PRs, or managing branch lineage
---

# Git Town

## Overview

Git Town automates high-level Git workflows, especially for **stacked pull requests**. Instead of manual git checkout/rebase/merge sequences, use git town commands that handle branching, syncing, and PR creation with proper branch lineage.

## When to Use

Use git town commands when:
- Creating feature branches (especially stacked changes)
- Syncing feature branches with main/master or parent branches
- Creating pull requests (git town sets correct target branch)
- Building incremental changes on top of unmerged work
- Repository has git town configured (check: `git town config`)

Don't use when:
- Repository doesn't have git town
- Doing low-level git operations (log, diff, blame)
- Working with commits directly (revert, cherry-pick)

## Stacked PRs Explained

**Stacked PRs** let you break large features into small, reviewable PRs that depend on each other. Instead of waiting for PR1 to merge before starting PR2, you create branches on top of branches.

**Key concepts:**
- **Branch lineage**: Git town tracks parent-child relationships
- **Automatic targeting**: `git town propose` creates PRs targeting the parent branch, not main
- **Propagating changes**: `git town sync` updates entire stack when parent changes

**Example stack:**
```
main
 └─ feature-1 (PR targets main)
     └─ feature-2 (PR targets feature-1)
         └─ feature-3 (PR targets feature-2)
```

## Quick Reference

| Task | Git Town Command | Instead of |
|------|------------------|------------|
| Create feature branch from main | `git town hack <name>` | `git checkout -b <name>` |
| Create stacked branch (child) | **`git town append <name>`** | `git town hack` or `git checkout -b` |
| Sync with parent/main | `git town sync` | `git fetch && git rebase origin/main` |
| Sync entire stack | `git town sync --stack` | Multiple manual rebases |
| Create PR (targets parent!) | **`git town propose`** | `gh pr create --base <parent>` |
| Show branch lineage | `git town status` | N/A |

**Critical distinctions:**
- **`hack` vs `append`**: Use `hack` only when branching from main. Use `append` for stacked branches.
- **`propose` vs `gh pr create`**: Use `propose` - it automatically sets the correct target branch based on lineage.

## Core Workflows

### Simple Feature Branch

```bash
# Start new feature from main
git town hack add-user-auth

# Make changes, commit normally
git add .
git commit -m "Add authentication"

# Sync with main (handles fetch, rebase, conflicts)
git town sync

# Create pull request (targets main)
git town propose
```

### Stacked PRs (Recommended for Large Features)

```bash
# Create first branch from main
git town hack refactor-auth-base

# Implement base refactoring, commit
git commit -m "Refactor authentication base"

# Create second branch ON TOP of first (stacked)
git town append add-oauth-support

# Implement OAuth, commit
git commit -m "Add OAuth support"

# Create third branch ON TOP of second
git town append add-oauth-tests

# Add tests, commit
git commit -m "Add OAuth tests"

# Sync entire stack with main
git town sync --stack

# Create PRs for each branch (in order)
git checkout refactor-auth-base
git town propose  # PR targets main

git checkout add-oauth-support
git town propose  # PR targets refactor-auth-base (parent)

git checkout add-oauth-tests
git town propose  # PR targets add-oauth-support (parent)
```

**When parent PR merges:** Run `git town sync` on child branches to update them.

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Using `git checkout -b` | Use `git town hack` (from main) or `git town append` (stacked) |
| Using `git town hack` for stacked branch | Use `git town append` - hack is only for branching from main |
| Using `gh pr create` | Use `git town propose` - automatically sets correct target branch |
| Manually specifying `--base` in PR | Use `git town propose` - respects lineage, no manual base needed |
| Manual rebase from main | Use `git town sync` - handles conflicts better |
| Manual updates when parent merges | Use `git town sync` - propagates changes through stack |
| Not checking if git town available | Run `git town config` first |
| Creating stack with manual branches | Use `git town append` - tracks lineage automatically |

## Key Benefits

**Stacked PRs:** Break large features into reviewable chunks without waiting for merges

**Automation:** One command replaces multi-step manual workflows

**Safety:** Built-in conflict handling and lineage tracking

**Consistency:** Same workflow across all git town repos

**Correct PR targeting:** `git town propose` automatically sets base branch to parent

## Configuration

Check repository configuration:
```bash
git town config  # Show all settings including branch lineage
```

View current branch lineage:
```bash
git town status  # Shows parent-child relationships
```

Most repos pre-configure git town. If not configured:
```bash
git town config setup  # Interactive setup
```

## Red Flags - You Should Use Git Town

- Writing `git checkout -b` when git town available
- Writing `git town hack` for a stacked branch (use `append` instead)
- Writing `gh pr create` when git town available (use `propose` instead)
- Manual `git fetch && git rebase origin/main` workflow
- Complex branch syncing with multiple git commands
- Manually setting PR base branch with `--base` for stacked changes
- Creating dependent branches without tracking lineage
- "I'll use standard git commands" when git town simplifies this
- "I'll use gh pr create" when git town propose handles targeting automatically

**Prefer git town commands over manual git workflows when available.** Git town handles edge cases (conflicts, lineage, PR targeting) that manual commands don't.
