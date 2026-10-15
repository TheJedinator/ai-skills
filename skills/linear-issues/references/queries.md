# Linear GraphQL query reference

## Contents

- Query patterns: assigned issues, sprint, team, cycles, triage, backlog, in progress, search, single issue, sprint progress
- Creating issues: prerequisites, single issue, sub-issues, batch tips, update, comment
- Adapting queries: filter and sort building blocks

Replace `<team>` with the team name from the viewer query in SKILL.md.

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

Use when: "team's issues", "what's the team working on", "<team> tickets"

Swap the team name as needed.

```graphql
{
  issues(first: 50, filter: {
    team: { name: { eq: "<team>" } }
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
    team: { name: { eq: "<team>" } }
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

```graphql
{
  issues(first: 50, filter: {
    team: { name: { eq: "<team>" } }
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
    team: { name: { eq: "<team>" } }
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
    team: { name: { eq: "<team>" } }
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
  issueSearch(query: "SSO", first: 20) {
    nodes {
      identifier title state { name type } team { name }
      assignee { name } url
    }
  }
}
```

### Single Issue Detail

Use when: the user references a specific ticket ID like "ENG-123"

```graphql
{
  issue(id: "<identifier>") {
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
  cycles(filter: { isActive: { eq: true }, team: { name: { eq: "<team>" } } }) {
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
"Sprint 19 (May 4–11): 3/8 done ✅, 4 in progress 🔄, 1 todo 📋"

## Creating Issues

Use when: "create a ticket", "file an issue", "make tickets for this work", "add to Linear"

### Prerequisites — Look Up IDs First

Before creating issues, you need the team ID, a state ID (for desired status), and optionally a project ID. Always query these rather than hardcoding.

**Get team ID and available states:**
```graphql
{
  teams(filter: { name: { eq: "<team>" } }) {
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
- Use `identifier` (e.g., "ENG-123") for display/reporting to the user
- Escape special characters in descriptions: `\n` for newlines, `\"` for quotes, `\u0027` for apostrophes in JSON strings

### Update an Issue

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

## Adapting Queries

These patterns cover the common cases. For unusual requests, compose filters by combining what's available:

- Filter by `assignee`: `{ assignee: { name: { eq: "Someone" } } }`
- Filter by `label`: `{ labels: { name: { eq: "bug" } } }`
- Filter by `priority`: `{ priority: { lte: 2 } }` (urgent + high)
- Filter by `dueDate`: `{ dueDate: { lt: "2026-05-10" } }`
- Sort: add `orderBy: updatedAt` to issue queries
- Filter by `project`: `{ project: { name: { eq: "Notifications" } } }`
