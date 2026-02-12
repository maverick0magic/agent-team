---
name: sprint
description: Manage sprint lifecycle — plan, dispatch agents, review outputs, and report progress. Auto-detects the correct phase if no argument is provided.
arguments:
  - name: phase
    description: "Sprint phase: plan, dispatch, review, or report (auto-detects if omitted)"
    required: false
user-invocable: true
---

# Sprint Management

You are running the **Sprint Management** command. This follows the Orchestrator Protocol's 4-phase cycle.

## Phase Detection

{{#if args.phase}}
Run the **{{args.phase}}** phase.
{{else}}
Auto-detect the correct phase by reading project state:

1. Read `.agents/orchestrator/sprint-plan.md`
2. Read `.agents/orchestrator/project-status.md`
3. Check each agent's `status.md`

**Detection logic:**
- If no sprint plan exists → `plan`
- If sprint plan exists but no agents are working and tasks are pending → `dispatch`
- If agents have completed work that hasn't been reviewed → `review`
- If all tasks are done → `report`
- If mixed state → present options to user
{{/if}}

---

## Phase: Plan

### Steps
1. Read `.agents/orchestrator/project-status.md` for current state
2. Read each hired agent's `tasks.md` for completed/pending work
3. Determine sprint number (increment from last sprint plan)
4. Propose sprint goals and tasks to the user:

```markdown
## Sprint [N] Proposal

**Goal**: [1-2 sentence goal]
**Duration**: [estimated timeframe]
**Autonomy**: [Low / Medium / High]

### Proposed Tasks
| ID | Agent | Task | Priority | Blocked By |
|----|-------|------|----------|------------|
[task rows]

### Parallelization Plan
[Which agents can work in parallel, which are sequential]
```

5. Wait for user approval
6. Write approved plan to `.agents/orchestrator/sprint-plan.md` using the sprint template
7. Create task entries in each agent's `tasks.md` (Pending section)

---

## Phase: Dispatch

### Steps
1. Read `.agents/orchestrator/sprint-plan.md` for current tasks
2. Identify tasks that are `pending` and not blocked
3. Group tasks by parallelizability:
   - **Parallel group**: Tasks with no dependencies on each other
   - **Sequential chain**: Tasks that depend on previous tasks

4. For each parallel group, spawn agents using the `Task` tool:
   - Set `model` per agent definition (inherit for Design/Coding/Codex, sonnet for Deploy/Content/Growth/QA)
   - Include in the prompt:
     - The agent's system prompt (from `agents/[name]-agent.md`)
     - Their specific task from the sprint plan
     - Relevant context files (CLAUDE.md, handoffs, specs)
   - Run parallel tasks concurrently

5. Update dispatched agents' `status.md` to `working`
6. Update task status to `in_progress` in each agent's `tasks.md`
7. Update sprint plan task status

### Dispatch Prompt Template
```
You are the [Agent Name] Agent. Read your agent definition for your full role and protocol.

**Your Task**: [task description from sprint plan]
**Task ID**: [S{N}-{code}{NN}]
**Sprint**: [N]

**Context Files to Read**:
- CLAUDE.md (project conventions)
- [relevant handoff files]
- [relevant spec files]

**Output**: Write your deliverable to [expected path]. Update your status.md and tasks.md when done.
```

---

## Phase: Review

### Steps
1. Read `.agents/orchestrator/sprint-plan.md` for expected deliverables
2. For each completed task:
   a. Read the agent's output/deliverable
   b. Verify it meets the task description and acceptance criteria
   c. Check for any new handoffs needed

3. **For code deliverables** (from Coding Agent):
   - Verify entry exists in `.agents/shared/handoffs/coding-to-codex.md` Review Queue
   - If Codex is hired, dispatch Codex Agent to review:
     - Spawn Codex with the coding-to-codex handoff and review checklist
     - Wait for review report
   - Update handoff status based on Codex verdict

4. **For design deliverables** (from Design Agent):
   - Check if design-to-coding handoff needs updating
   - Verify design tokens are complete

5. **Update project state**:
   - Update `.agents/orchestrator/project-status.md` with completed work
   - Update agent `status.md` files
   - Move completed tasks in `tasks.md`

6. **Report findings** to user:
```markdown
## Sprint [N] Review

### Completed
| Agent | Task | Deliverable | Status |
|-------|------|-------------|--------|
[rows]

### Codex Reviews
| Files | Verdict | Key Findings |
|-------|---------|-------------|
[rows if applicable]

### Needs Attention
[Any blocked tasks, failed reviews, or missing deliverables]

### Next Steps
[What should happen next — more dispatch, fixes, or report]
```

---

## Phase: Report

### Steps
1. Read all agent state files for comprehensive status
2. Compile sprint summary:

```markdown
## Sprint [N] Report

**Goal**: [sprint goal]
**Status**: [Complete / In Progress / Blocked]
**Duration**: [actual dates]

### Deliverables
| Agent | Tasks Planned | Tasks Done | Key Output |
|-------|--------------|------------|------------|
[rows]

### Key Metrics
- Tasks completed: [N/M]
- Codex reviews: [N passed, N with comments, N changes requested]
- Blockers resolved: [N]
- New blockers: [N]

### Autonomy Assessment
Current: [Low / Medium / High]
Recommendation: [Keep / Raise / Lower] — [reason]

### Retrospective
- **What worked**: [1-2 items]
- **What didn't**: [1-2 items]
- **Action items**: [improvements for next sprint]

### Next Sprint Preview
[Brief overview of what Sprint N+1 should focus on]
```

3. Ask user if they want to proceed to Sprint N+1 planning
