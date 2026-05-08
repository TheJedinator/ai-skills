---
name: linear-issues
description: Query and create Linear issues, manage sprints, triage, and team workload. Use whenever the user asks about their tickets, their team's work, sprint progress, triage queue, backlog, blocked items, or wants to create/update issues — even if they don't mention "Linear" explicitly.
---

# Linear Issues Skill

Query and mutate the Linear GraphQL API to answer questions about issues, sprints, team work, triage, and to create/update issues.

## Configuration

Before using this skill, ensure:
- `$LINEAR_API_KEY` is set as an environment variable (a Linear personal API key)
- You know your team name in Linear (used in team-scoped queries)

On first use, run the viewer query below to identify your user ID and team memberships.

## How to Use This Skill

When the user asks about their work, their team's work, sprints, or issues:

1. **Determine what they need** — map their request to one of the query patterns below
2. **Run the appropriate query** via `curl` against the Linear GraphQL API
3. **Present the results clearly** — group by state, use priority emoji, include Linear URLs

If the request is ambiguous (e.g., "what's going on?"), default to showing their assigned issues in the current sprint. If they mention a team without specifying which, use their primary team.

## API Mechanics

```bash
curl -s -X POST https://api.linear.app/graphql \
  -H "Content-Type: application/json" \
  -H "Authorization: $LINEAR_API_KEY" \
  -d '{"query": "<GRAPHQL_QUERY>"}' | python3 -m json.tool
```

If a query returns errors or empty results, tell the user what happened and suggest alternatives (e.g., "No triage issues found — want me to check the backlog instead?").

## Discovering User Context

Run this on first use to identify the user and their teams:

```graphql
{
  viewer {
    id name email
    teams { nodes { id name } }
  }
}
```

Cache the viewer ID (needed for mutations like assigning issues) and team names for subsequent queries.

## Key Concepts

- **Cycles** = sprints (typically 1-2 weeks)
- **State types**: `triage` → `backlog` → `unstarted` → `started` → `completed` / `canceled`
- **Started** includes sub-states like "In Progress", "In Review", "Blocked"
- **Priority**: 1=Urgent, 2=High, 3=Medium, 4=Low, 0=None

## Query Patterns

### My Assigned Issues (active/open)

Use when: "what am I working on", "my tickets", "my issues", "what's assigned to me"

```graphql
{
  viewer {
    assignedIssues(first: 50, filter: { state: { type: { nin: ["completed", "canceled"] } } }) {
      nodes {
        identifier title state { name type } priority priorityLabel
        labels { nodes { name } } cycle { number }
        team { name } dueDate estimate url createdAt updatedAt
      }
    }
  }
}
```

### My Issues in Current Sprint

Use when: "my sprint", "this cycle", "what's on my plate this week"

```graphql
{
  viewer {
    assignedIssues(first: 50, filter: {
      cycle: { isActive: { eq: true } }
      state: { type: { nin: ["completed", "canceled"] } }
    }) {
      nodes {
        identifier title state { name type } priority priorityLabel
        labels { nodes { name } } team { name } dueDate estimate url
      }
    }
  }
}
```

### Team Issues (all active)

Use when: "team's issues", "what's the team working on"

Replace `"My Team"` with the actual team name.

```graphql
{
  issues(first: 50, filter: {
    team: { name: { eq: "My Team" } }
    state: { type: { nin: ["completed", "canceled"] } }
  }) {
    nodes {
      identifier title state { name type } priority priorityLabel
      assignee { name } labels { nodes { name } }
      cycle { number } dueDate estimate url
    }
  }
}
```

### Team Sprint Issues

Use when: "team's sprint", "what's in this cycle for the team"

```graphql
{
  issues(first: 50, filter: {
    team: { name: { eq: "My Team" } }
    cycle: { isActive: { eq: true } }
    state: { type: { nin: ["completed", "canceled"] } }
  }) {
    nodes {
      identifier title state { name type } priority priorityLabel
      assignee { name } labels { nodes { name } }
      dueDate estimate url
    }
  }
}
```

### Current Active Cycles (Sprints)

Use when: "what sprint are we on", "when does the cycle end", "sprint dates"

```graphql
{
  cycles(filter: { isActive: { eq: true } }) {
    nodes {
      id name number startsAt endsAt
      team { name }
      issues { nodes { identifier title state { name type } assignee { name } } }
    }
  }
}
```

### Triage Issues

Use when: "triage", "what needs triaging", "untriaged tickets"

These are issues that have landed but haven't been categorized or assigned yet — they need someone to look at them and decide priority/owner.

```graphql
{
  issues(first: 50, filter: {
    team: { name: { eq: "My Team" } }
    state: { type: { eq: "triage" } }
  }) {
    nodes {
      identifier title state { name } priority priorityLabel
      assignee { name } createdAt labels { nodes { name } } url
    }
  }
}
```

### Backlog Issues

Use when: "backlog", "what's queued up", "upcoming work"

```graphql
{
  issues(first: 50, filter: {
    team: { name: { eq: "My Team" } }
    state: { type: { eq: "backlog" } }
  }) {
    nodes {
      identifier title priority priorityLabel
      assignee { name } labels { nodes { name } } createdAt url
    }
  }
}
```

### In Progress / Blocked Issues

Use when: "what's in progress", "blocked issues", "what's actively being worked on"

Note: "started" type includes In Progress, In Review, and Blocked states. Filter by state name if you need just one.

```graphql
{
  issues(first: 50, filter: {
    team: { name: { eq: "My Team" } }
    state: { type: { eq: "started" } }
  }) {
    nodes {
      identifier title state { name } priority priorityLabel
      assignee { name } labels { nodes { name } } url updatedAt
    }
  }
}
```

### Search Issues by Keyword

Use when: the user mentions a specific topic, error, or feature name

```graphql
{
  issueSearch(query: "search term here", first: 20) {
    nodes {
      identifier title state { name type } team { name }
      assignee { name } url
    }
  }
}
```

### Single Issue Detail

Use when: the user references a specific ticket ID like "TEAM-123"

```graphql
{
  issue(id: "TEAM-123") {
    identifier title description state { name type }
    priority priorityLabel assignee { name }
    labels { nodes { name } } comments { nodes { body user { name } createdAt } }
    cycle { number } team { name } url createdAt updatedAt dueDate
  }
}
```

### Sprint Progress Summary

Use when: "how's the sprint going", "sprint progress", "are we on track"

```graphql
{
  cycles(filter: { isActive: { eq: true }, team: { name: { eq: "My Team" } } }) {
    nodes {
      number startsAt endsAt
      issues {
        nodes { identifier title state { name type } assignee { name } estimate }
      }
    }
  }
}
```

After fetching, group issues by state type and present a summary like:
"Sprint 5 (May 4–11): 3/8 done ✅, 4 in progress 🔄, 1 todo 📋"

## Creating Issues

Use when: "create a ticket", "file an issue", "make tickets for this work", "add to Linear"

### Prerequisites — Look Up IDs First

Before creating issues, you need the team ID, a state ID (for desired status), and optionally a project ID. Always query these rather than hardcoding.

**Get team ID and available states:**
```graphql
{
  teams(filter: { name: { eq: "My Team" } }) {
    nodes {
      id name key
      states { nodes { id name type } }
    }
  }
}
```

**Get project ID (if associating with a project):**
```graphql
{
  projects(filter: { name: { containsIgnoreCase: "project name" } }) {
    nodes { id name state }
  }
}
```

Choose projects with `state: "started"` (not "completed" or "canceled").

### Create a Single Issue

```graphql
mutation {
  issueCreate(input: {
    teamId: "<team-uuid>"
    stateId: "<state-uuid>"
    projectId: "<project-uuid>"
    title: "Issue title"
    description: "Markdown description"
  }) {
    success
    issue { id identifier url }
  }
}
```

**Key input fields:**
- `teamId` (required): UUID of the team
- `title` (required): Issue title
- `description`: Markdown-formatted body
- `stateId`: UUID of desired state (e.g., Backlog, Todo). Query team states to find the right one.
- `projectId`: UUID of the project to associate with
- `parentId`: UUID of parent issue (creates a sub-issue)
- `priority`: 1=Urgent, 2=High, 3=Medium, 4=Low, 0=None
- `assigneeId`: UUID of the assignee
- `labelIds`: Array of label UUIDs
- `estimate`: Story point estimate (number)

### Create Sub-issues (Parent/Child Hierarchy)

To create a sub-issue, include `parentId` pointing to the parent issue's UUID:

```graphql
mutation {
  issueCreate(input: {
    teamId: "<team-uuid>"
    stateId: "<state-uuid>"
    projectId: "<project-uuid>"
    parentId: "<parent-issue-uuid>"
    title: "Sub-issue title"
    description: "Work details"
  }) {
    success
    issue { id identifier url }
  }
}
```

**Workflow for creating parent + sub-issues:**
1. Create the parent issue first — capture its `id` from the response
2. Create each sub-issue with `parentId` set to the parent's `id`
3. All issues inherit the project from the parent, but always pass `projectId` explicitly for clarity

### Batch Creation Tips

- Create all parents first, then all sub-issues (you need parent IDs)
- Linear's API is one-mutation-per-request (no batch endpoint) — make sequential calls
- Always capture the `id` field from responses (not `identifier`) — `id` is the UUID used in subsequent mutations
- Use `identifier` (e.g., "ENG-42") for display/reporting to the user
- Escape special characters in descriptions: `\n` for newlines, `\"` for quotes, `\u0027` for apostrophes in JSON strings

### Common State Types

Always query fresh, but for reference:
- **Backlog** (type: `backlog`): Use for new planned work
- **Todo** (type: `unstarted`): Use for work ready to start
- **Triage** (type: `triage`): Use for incoming/unprocessed issues

## Updating Issues

### Update Fields

```graphql
mutation {
  issueUpdate(id: "<issue-uuid>", input: {
    stateId: "<new-state-uuid>"
    assigneeId: "<user-uuid>"
    priority: 2
  }) {
    success
    issue { id identifier url state { name } }
  }
}
```

To find a state UUID by name:
```graphql
{ workflowStates(filter: { team: { name: { eq: "My Team" } }, name: { eq: "In Progress" } }) { nodes { id name } } }
```

### Add a Comment

```graphql
mutation {
  commentCreate(input: {
    issueId: "<issue-uuid>"
    body: "Comment in markdown"
  }) {
    success
    comment { id }
  }
}
```

## Presenting Results

Format results so they're scannable at a glance:

- **Group by state**: In Progress → In Review → Todo → Backlog
- **Priority emoji**: 🔴 Urgent, 🟠 High, 🟡 Medium, 🔵 Low, ⚪ None
- **Always include** the Linear URL so the user can click through
- **For team views**, group by assignee when there are many issues
- **For sprint progress**, lead with the summary line, then show details

**Example output for "what's in my sprint":**

> **Sprint 5** (May 4–11) — Engineering
>
> 🔄 **In Progress**
> - 🟠 ENG-42 — Fix payment retry logic [→ link]
> - ⚪ ENG-38 — Update onboarding flow [→ link]
>
> 📋 **Todo**
> - ⚪ ENG-45 — Add rate limiting to webhook endpoint [→ link]

## Adapting Queries

These patterns cover the common cases. For unusual requests, compose filters by combining what's available:

- Filter by `assignee`: `{ assignee: { name: { eq: "Someone" } } }`
- Filter by `label`: `{ labels: { name: { eq: "bug" } } }`
- Filter by `priority`: `{ priority: { lte: 2 } }` (urgent + high)
- Filter by `dueDate`: `{ dueDate: { lt: "2026-05-10" } }`
- Sort: add `orderBy: updatedAt` to issue queries
- Filter by `project`: `{ project: { name: { eq: "My Project" } } }`
