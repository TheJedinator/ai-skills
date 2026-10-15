---
name: linear-issues
description: Query and create Linear issues, manage sprints, triage, and team workload. Use whenever the user asks about their tickets, their team's work, sprint progress, triage queue, backlog, blocked items, or wants to create/update issues — even if they don't mention "Linear" explicitly.
---

# Linear Issues Skill

Query and mutate the Linear GraphQL API to answer questions about issues, sprints, team work, triage, and to create/update issues.

## How to Use This Skill

When the user asks about their work, their team's work, sprints, or issues:

1. **Determine what they need** — map their request to a pattern in `references/queries.md`
2. **Run the appropriate query** via `curl` against the Linear GraphQL API
3. **Present the results** — group by state, include Linear URLs

If the request is ambiguous (e.g., "what's going on?"), default to showing their assigned issues in the current sprint. If they mention a team without specifying which, use the primary team from the viewer query.

## API Mechanics

```bash
curl -s -X POST https://api.linear.app/graphql \
  -H "Content-Type: application/json" \
  -H "Authorization: $LINEAR_API_KEY" \
  -d '{"query": "<GRAPHQL_QUERY>"}' | python3 -m json.tool
```

The `$LINEAR_API_KEY` environment variable is already set. If a query returns errors or empty results, tell the user what happened and suggest alternatives (e.g., "No triage issues found — want me to check the backlog instead?").

## User Context

Resolve identity once per session, never assume it:

```graphql
{ viewer { id name email teams { nodes { id name key } } } }
```

The user's primary team is the first team returned unless they name another. Every team-scoped query below takes the team name from this result.

## Key Concepts

- **State types**: `triage` → `backlog` → `unstarted` → `started` → `completed` / `canceled`
- **Started** includes sub-states like "In Progress", "In Review", "Blocked"
- **Priority**: 1=Urgent, 2=High, 3=Medium, 4=Low, 0=None

## Queries and mutations

Every GraphQL pattern lives in [references/queries.md](references/queries.md): read-only queries (assigned, sprint, team, triage, backlog, search, single issue, sprint progress), mutations (create, sub-issues, update, comment), and filter building blocks. Read the section you need, substitute `<team>` and IDs, then run it through the curl above.

After any mutation, re-query the issue by `id` and confirm the fields you set. If a field did not take, report it instead of assuming success.

## Presenting Results

Format results so they're scannable at a glance:

- **Group by state**: In Progress → In Review → Todo → Backlog
- **Priority**: show the `priorityLabel`; emoji are optional
- **Always include** the Linear URL so the user can click through
- **For team views**, group by assignee when there are many issues
- **For sprint progress**, lead with the summary line, then show details

**Example output for "what's in my sprint":**

> **Sprint <n>** (<dates>) — <team>
>
> 🔄 **In Progress**
> - 🟠 <TEAM>-123 — <title> [→ link]
>
> 📋 **Todo**
> - ⚪ <TEAM>-124 — <title> [→ link]

