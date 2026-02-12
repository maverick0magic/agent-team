---
name: status
description: Read-only team dashboard showing all agent statuses, task counts, and blockers. Never modifies any files.
arguments:
  - name: agent
    description: "Specific agent name for detailed view (e.g., 'coding', 'codex'). Omit for full team overview."
    required: false
user-invocable: true
---

# Team Status Dashboard

You are generating a **read-only status dashboard**. This command NEVER modifies any files — it only reads and reports.

{{#if args.agent}}
## Detailed Agent View: {{args.agent}}

Read the following files for the **{{args.agent}}** agent:
1. `.agents/{{args.agent}}/status.md`
2. `.agents/{{args.agent}}/tasks.md`
3. Any recent files in `.agents/{{args.agent}}/reviews/` (if applicable)

Present a detailed view:

```markdown
## {{args.agent}} Agent — Detailed Status

**Status**: [idle / working / blocked]
**Last Updated**: [date from status.md]

### Current Task
[What they're working on, or "None" if idle]

### Task Summary
| Category | Count |
|----------|-------|
| In Progress | [N] |
| Pending | [N] |
| Completed | [N] |
| Total | [N] |

### Recent Tasks
[Last 5 completed tasks with dates and verdicts]

### Pending Tasks
[All pending tasks with priorities and blockers]

### Blockers
[Any blocked tasks with what they're waiting on, or "None"]
```

{{else}}
## Full Team Dashboard

Read the following for every agent directory found in `.agents/`:
1. Each agent's `status.md`
2. Each agent's `tasks.md`
3. `.agents/orchestrator/sprint-plan.md` (for sprint context)
4. `.agents/orchestrator/project-status.md` (for overall health)

### Step 1: Read Sprint Context

Read the current sprint plan to understand what should be happening.

### Step 2: Read All Agent States

For each agent directory in `.agents/` (excluding `orchestrator` and `shared`):
- Read `status.md` for current state
- Read `tasks.md` and count tasks by category (in progress, pending, completed)
- Note any blockers

### Step 3: Present Dashboard

```markdown
# Agent Team Dashboard
**Sprint**: [N] — [Sprint Name]
**Sprint Health**: [On Track / At Risk / Blocked]
**Last Updated**: [now]

## Team Overview

| Agent | Status | In Progress | Pending | Done | Blockers |
|-------|--------|-------------|---------|------|----------|
| Orchestrator | active | — | — | — | — |
| Design | [status] | [N] | [N] | [N] | [N or —] |
| Coding | [status] | [N] | [N] | [N] | [N or —] |
| Codex | [status] | [N] | [N] | [N] | [N or —] |
| Deploy | [status] | [N] | [N] | [N] | [N or —] |
| Content | [status] | [N] | [N] | [N] | [N or —] |
| Growth | [status] | [N] | [N] | [N] | [N or —] |
| QA | [status] | [N] | [N] | [N] | [N or —] |

## Sprint Progress

**Tasks**: [completed]/[total] ([percentage]%)
**Acceptance Criteria**: [N met]/[N total]

### Progress Bar
[████████░░░░░░░░] 50%

## Active Blockers
[List of any blocked tasks across all agents, or "None — all clear"]

## Recent Activity
[Last 5 task completions across all agents, sorted by date]

## Handoff Queue
| Handoff | From → To | Items Waiting | Status |
|---------|-----------|---------------|--------|
| Design → Coding | [N] items | [ready / empty] |
| Coding → Codex | [N] items | [in review / empty] |
| Coding → Deploy | [N] items | [ready / empty] |
```

### Step 4: Health Assessment

If there are blockers or agents that appear stuck:
- Flag the issue
- Suggest resolution (e.g., "Codex has 3 items waiting — run `/agent-team:sprint dispatch` to process")

If everything is on track:
- Confirm healthy state
- Suggest next action based on sprint phase
{{/if}}

## Key Rules
- This command is **read-only** — never write, edit, or create files
- If `.agents/` doesn't exist, report: "No agent team found. Run `/agent-team:kickoff` to set up."
- Handle missing files gracefully — report "not found" rather than erroring
- Use the agent's display name (capitalized) in output, not directory name
