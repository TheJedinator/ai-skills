---
name: babysit-pr-stack
description: Own a chain of stacked PRs until every PR merges. Merge in strict order, rebase the stack after each squash merge, answer review comments, fix CI, find missing code-owner approvals and ask for them in Slack. Use when the user asks you to babysit, watch, drive, land or merge a PR stack or PR chain, or says "merge these in order", "keep the stack rebased" or "get approvals for the stack".
---

# Babysit a PR Stack

A stack is a chain of PRs. Each PR's branch contains the branch under it. The chain merges bottom-up, in a fixed order, with squash merges. Your job is to keep every PR mergeable and to merge each one when its gates pass. You do this for days, through many wake-ups.

**Never change the merge order.** The user ordered the chain for a reason. Do not merge a higher PR because it is ready first.

## Setup

1. **Register the stack with `gh stack`.** Do this in a dedicated clone or worktree with a clean working tree. `gh stack` rebases the branches it has checked out, so do not share the user's working checkout.
   ```bash
   gh extension install github/gh-stack     # once; needs v0.2.0 or later: `gh extension upgrade stack`
   gh stack checkout <bottom-pr-url>        # rebuild the stack locally from GitHub
   gh stack view --short                    # order, branches, PR numbers
   ```
   If the PRs are not a GitHub stack yet, adopt the branches in merge order with `gh stack init <branch-1> <branch-2> ...`, then `gh stack submit`. `gh stack link <pr> <pr> ...` does the same for existing PRs, in bottom-to-top order, without local tracking.

   Read `gh stack view` and check that the order matches the user's merge order. Then run `gh stack rebase` while the chain is clean, before the first merge. Any branch that moves did not sit on the branch below it. Stop and ask.

2. **Subscribe to every PR.** Call `subscribe_pr_activity` once per PR, not only for the bottom PR. A review on a higher PR also needs an answer. Then end the turn. Events wake you. Do not poll with `sleep`.

3. **Map code-owner gaps up front.** Most of the waiting time is approvals, not CI. See [Code-owner approvals](#code-owner-approvals). Delegate the mapping to a background subagent, because it reads hundreds of changed files.

4. **Learn the tool surface.** Check what you have before you depend on it:
   - **GitHub:** use the `gh` CLI with the `gh-stack` extension for stack operations. Some runners have no `gh` CLI. There, use plain `git` for rebases, and the `mcp__github__*` tools for the rest:
     - `pull_request_read` with the methods `get`, `get_reviews`, `get_review_comments`, `get_status` and `get_files`
     - `merge_pull_request`, `resolve_review_thread`, `add_reply_to_pull_request_comment`, `add_issue_comment`, `get_team_members` and `update_pull_request`
   - **CI:** you may not be able to re-run a CI job. If you cannot, say so once in your report. Do not push an empty commit to start CI.
   - **Slack:** use the `mcp__Slack__*` tools, if they are connected.

## The merge loop

Do this each time the bottom open PR may be ready: a review arrives, CI finishes, or the user asks.

1. **Check the gates on the current head.** All four must pass:
   - CI is green on the head SHA, not on an older commit. CI takes several minutes after a push.
   - Every review thread is resolved. GitHub refuses the merge with `405 All comments must be resolved` otherwise. This includes bot threads.
   - Every code-owner team has an approval from one of its own members. An approval from a different team does not count.
   - There is no merge conflict, and the base is the default branch (`master` or `main`).
2. **Merge it.** Call `merge_pull_request`, not `gh stack merge`. The second cannot pin the head SHA or set the squash title. Use these settings:
   - `merge_method: squash`
   - `expectedHeadSha`: the head you checked. Without it, a push that lands after your check gets merged without review.
   - `commit_title`: the commit subject plus ` (#<pr>)`, for example `Add widget model (#101)`. Copy the subject from `git log -1 --format=%s origin/<branch>`. Do not type it from memory: a squash title on the default branch cannot be fixed later.
3. **If the merge fails, read the 405 message.** It names the gate:

   | Message | Action |
   |---|---|
   | `All comments must be resolved` | Find the open threads and answer them. See [Review comments](#review-comments). |
   | `Required status check ... pending` | CI has not finished. Wait for the `check_suite.completed` event. |
   | `Waiting on code owner review from <team>` | That team has no approval yet. Ask in Slack. See [Asking for approvals](#asking-for-approvals). |
   | A head-SHA mismatch | Someone pushed after your check. Check the gates again on the new head. |

4. **Restack the next PR.** See the next section.
5. **Check the base of the next PR.** It must now be the default branch. GitHub does not rebase or restack for you. Call `update_pull_request` with `base` set to the default branch, after the rebase and push.

## Restacking

A squash merge puts one new commit on the default branch. The branches above the merged PR still contain the old, unsquashed commits. So the next PR becomes conflicted or shows a doubled diff.

`gh stack` tracks the chain on GitHub and in the clone, so no stack file is needed. `gh stack rebase` rebases each branch onto the new tip of the branch below it, from the trunk upward. When a branch's PR is merged, it switches to `--onto` mode on its own, so the squash commit does not conflict with the old commits.

**After each merge, rebase and push only the next PR.** Every push re-runs the PR's CI. Some checks, such as Danger, fetch the before and after contents of each changed file, so one run on a large PR can cost hundreds of GitHub API calls. A full restack of a large stack, repeated a few times in an hour, can use up the repo's hourly Actions API quota. Then those checks fail on every PR in the repo, not only the stack's.

```bash
gh stack checkout <next-branch>              # the next PR's branch
gh stack rebase --downstack                  # fetch, then rebase trunk..this branch only; pushes nothing
git push --force-with-lease=<branch>:<old-sha> origin <branch>   # old-sha: the remote tip you read before the rebase
```

Higher PRs keep their old base until they become next. Their diffs can show extra changes for a while. Their approvals stay.

Skip the rebase when the next PR already merges cleanly as it is: no conflict, and branch protection does not require an up-to-date branch. Still retarget its base.

Other commands:

| Command | Use |
|---|---|
| `gh stack sync` | Fetch, fast-forward trunk, rebase, push **every** branch, refresh PR state. Push count is the whole stack, so use it only when the user asks or the stack is small. It leaves all branches unchanged if a rebase conflicts. |
| `gh stack sync --prune` | Same, and delete local branches of merged PRs. Run it after the stack lands. |
| `gh stack rebase --upstack` | Rebase the current branch and everything above it. Use it after you commit a fix on a lower branch. |
| `gh stack push` | Push every active branch, each with its own `--force-with-lease`. The push is not atomic: fix a rejected branch and run it again. |
| `gh stack rebase --abort` | Restore every branch to its state before the rebase. |

Do not use `git range-diff` to check a rebase. It flags hunk-header context that moves after every merge, and that is not a real change. Compare the `+`/`-` lines of `git diff <old-tip>~k <old-tip>` and `git diff <new-tip>~k <new-tip>` instead, where `k` is the number of commits the branch owns (`git rev-list --count <branch-below>..<tip>`).

A push uses `--force-with-lease`, pinned to the tip you read. It fails if a human pushed to the branch meanwhile. Then fetch, look at their commit and decide.

**Without `gh` or the extension,** do the same by hand. `<merged-head>` is the head SHA you passed as `expectedHeadSha`.

```bash
git fetch origin
git rebase --onto origin/<base> <merged-head> <next-branch>
git push --force-with-lease=<next-branch>:<old-sha> origin <next-branch>
```

The extension keeps its state in the clone. A runner can move or restart during a long babysit, and then the clone is gone. `gh stack checkout <pr-url>` rebuilds the state from GitHub. Use a URL, because a bare number is read as a stack number first. Do not keep chain state anywhere else.

**When a rebase conflicts,** `gh stack rebase` pauses and prints the conflicted files. A common cause is a stamp line that the default branch already fixed. For example, the default branch renamed `get_logger()` to `get_logger("Service")`, and the branch adds `# noqa: RULE001` to the old line. The violation no longer exists, so keep the default branch's side (`git checkout --ours <file>` during a rebase). Do this only when that hunk is the branch's only change in the file. Check with `git show <branch-tip> -- <file>`. Then `git add` the files and run `gh stack rebase --continue`. Say in your report which stamps were dropped, because that PR's diff changes.

Other conflict cases:

- **The default branch deleted the file** (`DU` in `git status`). Keep the deletion with `git rm <file>`.
- **The default branch changed the line so it no longer breaks the rule.** For example, `IntegerField` became `BigIntegerField`. Keep its side.
- **The default branch added new code that breaks the rule.** The rebase may be clean, but CI lint fails. After every rebase, run the repo's linter. Add `# noqa: <CODE>` to each new hit and amend that into the PR's commit.
- When the stamp count changes, update the count in the PR body. Copy the body, change only the numbers, and send the whole body back. Count with `git diff origin/<base> <new-tip> | grep -c '^+.*noqa:.*<CODE>'`.
- `--continue` rebases the rest of the chain locally. Push only the next PR's branch. A conflict further up the chain is fixed when that PR becomes next.

**Add a fix commit to a lower branch** with `gh stack down` or `gh stack checkout <branch>`, commit there, then `gh stack rebase --upstack`. The extension keeps the commit, with no count to update. Push the changed branches.

## Review comments

Read every human comment and every bot comment on every PR, not only on the bottom PR. Remediate or refute each comment based on evidence:

- **A correct finding:** fix it on the branch that owns the code. Run the repo's checks for that branch, commit, and run `gh stack rebase --upstack`.
- **A wrong finding:** reply with the evidence, such as a file and line, a doc quote or a test result. Then resolve the thread.
- **A repeated bot finding with a standing answer:** reply once with the standing answer, then resolve the thread. Re-review bots (AI code reviewers, for example) review every push again. They often reopen the same point on new lines, and each open thread blocks the merge.
- **A design question from a human:** answer it. If the answer changes scope, ask the user first.

After a large restack, a subagent can sweep the remaining open threads on all PRs. Give it the standing answers. Tell it to resolve only the threads that are already answered.

## CI failures

- **First, rule out a failure that is not this PR's.** Is it also red on the default branch? Does it name a service that the diff does not touch? Is it an infra step (checkout, install, runner loss)?
- **A test that fails only sometimes is a bug, not bad luck.** Find the root cause. A real case was `factory.Sequence("Vendor {n}")`, which produced "Vendor 1234" and matched a search test for `"1234"`. Prove it before you fix it: force the condition, for example `Factory.reset_sequence(1234)`, and see the test fail. Then pin the data, and see it pass. Fix it on the lowest branch where it fails, then restack. Report flakes in unrelated code to the user. Do not widen the PR.
- **Never** skip, disable or quarantine a test. Never push an empty commit. Never close and reopen a PR to start CI.

## Code-owner approvals

Map, for each PR, the teams that still need to approve:

1. Read `CODEOWNERS` from the default branch. A rule with no owner makes a path unowned. The last matching rule wins.
2. List each PR's changed files with `pull_request_read` method `get_files`. Read every page. A big PR has more than 100 files.
3. Match each file to the teams that own it. That gives the owning teams for each PR.
4. Get each team's members with `get_team_members`.
5. Get each PR's reviews. A team is satisfied when one of its own members approved, and no later `CHANGES_REQUESTED` or dismissal from the same person cancels it. A later `COMMENTED` review does not cancel an approval.
6. Make a table: PR, owning teams, teams still missing, current approvers. Also list each team's missing PRs. One approval request per team then covers many PRs.

Caveats to report:
- If the repo dismisses stale approvals, a restack push can remove an approval. The next merge attempt shows it. Check the repo's branch protection settings.
- If `CODEOWNERS` changes on the default branch during the babysit, the owning teams can change.

## Asking for approvals

- **Ask each team once, in its own channel.** List all of the team's PRs, in merge order, as links. Say what the change is and why review is quick (for example, "most changes are `# noqa` comments").
- **If the user is not a member of the team channel,** post in a general channel of that team that the user is a member of.
- **Mark every post** with "_Posted by Claude on <user>'s behalf._" Your Slack posts appear under the user's name.
- **Send one reminder per team,** as a reply in the same thread, when the chain reaches that team's first PR. Name the team members who can approve. Do not send more. Ask the user to ping a person directly.
- **Before you post, search** for your earlier posts in the channel, so that you do not post twice.

## Handling the event flood

One restack of 20 PRs creates hundreds of events. A deploy bot may edit a preview-environment comment on every PR each time any PR deploys. Classify each event quickly:

| Event | Action |
|---|---|
| Preview-environment comment edited, bot summary table edited | None |
| `check_suite.completed` on a PR that is not next to merge | None. CI on the higher PRs only matters when they become next. |
| `check_suite.completed` on the next PR | Check the merge gates. |
| CI finishes but no event arrives | Expected for CI systems that report through commit statuses, which do not wake the session. `check_suite.completed` can follow a few minutes later. Wait for it; do not schedule check-ins. |
| A preview-environment check fails or is still pending | Not a required check. Merge when the required CI checks have passed. |
| Merge returns 500 | A GitHub fault. Read the PR. If it is still open, retry the merge once. |
| A GitHub Actions check fails with `API rate limit exceeded for installation` | The repo's hourly Actions API quota is used up. The log shows `x-ratelimit-reset`. Stop all pushes until then. If a merge returns 405 on one of these checks, re-run it after the reset. |
| Preview-environment `sync failed` status | Comment once per PR that it is a preview-environment failure and does not block the merge. Do not comment again. |
| Bot review comment | See [Review comments](#review-comments). |
| Human review or comment | Act on it. This is the only event that is usually real work. |
| Your own reply echoed back | None |
| An event with an old `queued at` time, or a SHA that is not the current head | Usually none. A queue can redeliver hours of old events in a batch. Check the PR's current state before you act on one. |

Drain `ReadNotifications` until it reports 0 remaining. Then give the user one short line: what changed, and what the chain waits on. Do not narrate each event.

## Runner gotchas

- **SHAs change after each turn.** A stop hook may re-sign commits on agent-created branches. Read `git log` again before you use a SHA in a comment or as `expectedHeadSha`.
- **The network can reset mid-command.** A `git fetch`, `git push` or `uv sync` can fail once with a 504 or a TLS reset. Retry with backoff (2, 4, 8, 16 seconds).
- **A new session can need fresh push permission.** Auto mode can treat a force-push to someone else's branch as a change to a shared resource, and block it. Ask the user once to allow `git push --force-with-lease` for the stack's branches.
- **Long work runs in the background.** Use a background subagent for owner mapping and thread sweeps. The user can talk to you while they run.
- **Weekends and nights are quiet.** When the chain waits only on people, say so once. Do not recheck on every bot event.

## Reporting

End each run with three headings, and leave out a heading that is empty:
- **Blocked on me:** decisions only the user can make, and people only the user can ping.
- **Changed:** merges, pushes, fixes, posts.
- **Found:** flakes, gate surprises, things you could not verify.

Use full PR links (`https://github.com/<owner>/<repo>/pull/<n>`), not bare `#123`.
