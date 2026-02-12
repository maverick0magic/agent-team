---
name: kickoff
description: Initialize a multi-agent team for the current project. Classifies project type, assesses codebase, proposes a team, scaffolds .agents/ directory, and writes Sprint 1 plan.
arguments:
  - name: type
    description: "Project type: personal or work (will ask if not provided)"
    required: false
user-invocable: true
---

# Agent Team Kickoff

You are initiating the **Agent Team Kickoff Protocol**. Follow each step in order.

## Step 1: Classify Project Type

{{#if args.type}}
The user specified project type: **{{args.type}}**.
{{else}}
Ask the user:

> **Is this a personal project or a work project?**
>
> | Type | Team Size | Implications |
> |------|-----------|-------------|
> | **Personal** | 2-4 agents | Lean team, move fast, skip formal reviews |
> | **Work** | 5-7 agents | Full team, Codex mandatory, structured sprints |

Wait for their answer before proceeding.
{{/if}}

## Step 2: Assess the Codebase

Read the following files (skip any that don't exist):
1. `CLAUDE.md` — project instructions
2. `README.md` — project overview
3. `package.json` — Node.js dependencies and scripts
4. `requirements.txt` or `pyproject.toml` — Python dependencies
5. `Cargo.toml` — Rust dependencies
6. `go.mod` — Go dependencies
7. `Gemfile` — Ruby dependencies
8. `pubspec.yaml` — Flutter/Dart dependencies
9. `build.gradle` or `pom.xml` — Java/Kotlin dependencies
10. `docker-compose.yml` or `Dockerfile` — containerization

Also run `ls` on the project root to understand the directory structure.

From this, determine:
- **Tech stack** (languages, frameworks, databases, hosting)
- **Project maturity** (greenfield, early, mid, mature)
- **Has frontend** (yes/no)
- **Has backend** (yes/no)
- **Existing tests** (yes/no, which framework)

## Step 3: Present Team Hiring Proposal

Based on the assessment and project type, present this proposal:

```
TEAM PROPOSAL for [Project Name]
Type: [Personal / Work]
Stack: [detected tech stack]
Maturity: [greenfield / early / mid / mature]

Proposed Team:
  Lead Agent (Orchestrator) ........... YOU
  Coding Agent ........................ [HIRE / SKIP] — [reason]
  Design Agent ........................ [HIRE / SKIP] — [reason]
  Codex (Code Review) ................. [HIRE / SKIP] — [reason]
  Deploy Agent ........................ [HIRE / SKIP] — [reason]
  Content Agent ....................... [HIRE / SKIP] — [reason]
  Growth & Marketing Agent ............ [HIRE / SKIP] — [reason]
  QA Agent ............................ [HIRE / SKIP] — [reason]

Rationale: [1-2 sentences on why this team composition fits]
```

### Default Team Sizes

**Personal projects** — hire 2-4 agents:
- Always: Coding
- If frontend: Design
- If deploying: Deploy
- Consider: QA (if complex)
- Usually skip: Codex, Content, Growth

**Work projects** — hire 5-7 agents:
- Always: Coding, Design, Codex, Deploy, QA
- If user-facing: Content, Growth
- Codex is **mandatory** for work projects

**Wait for user approval before proceeding.**

## Step 4: Scaffold .agents/ Directory

After the user approves the team, create the directory structure:

```
.agents/
├── orchestrator/
│   ├── sprint-plan.md
│   ├── project-status.md
│   └── decision-log.md
├── shared/
│   └── handoffs/
│       ├── design-to-coding.md      (if Design hired)
│       ├── coding-to-codex.md       (if Codex hired)
│       └── coding-to-deploy.md      (if Deploy hired)
```

For each **hired agent**, create:
```
.agents/[agent-name]/
├── status.md      — contains: "Status: idle"
└── tasks.md       — contains: empty task table with headers
```

Agent directory names: `design`, `coding`, `codex`, `deploy`, `content`, `growth`, `qa`

### status.md template:
```markdown
# [Agent Name] Agent — Status
**Current**: idle
**Last Updated**: [YYYY-MM-DD]
```

### tasks.md template:
```markdown
# [Agent Name] Agent — Tasks

## In Progress
| ID | Task | Started | Notes |
|----|------|---------|-------|

## Pending
| ID | Task | Priority | Blocked By |
|----|------|----------|------------|

## Completed
| ID | Task | Completed | Verdict |
|----|------|-----------|---------|
```

## Step 5: Generate Stack-Specific Review Checklist

If Codex is hired, read the generic checklist from the plugin's `skills/orchestrator/review-checklist.md` and customize the stack-specific placeholder comments for the detected tech stack.

Write the customized checklist to `.agents/codex/review-checklist.md`.

## Step 6: Write Sprint 1 Plan

Using the template from `skills/orchestrator/sprint-template.md`, write the first sprint plan to `.agents/orchestrator/sprint-plan.md`.

Sprint 1 should typically focus on:
- **Design**: Design system tokens, screen inventory, Figma setup
- **Deploy**: Environment scaffold, CI/CD skeleton
- **Content**: Initial content strategy, copy inventory
- **Growth**: Growth strategy, user journey, metrics/KPIs
- **QA**: Master test plan, test cases for first features
- **Coding**: Project scaffold (if greenfield) or orientation (if existing)

## Step 7: Write Project Status

Write initial `.agents/orchestrator/project-status.md`:

```markdown
# Project Status — [Name]

**Last Updated**: [YYYY-MM-DD]
**Current Sprint**: Sprint 1 — [Sprint Name]
**Overall Health**: Starting

## Summary
Project kickoff complete. Team of [N] agents hired. Sprint 1 plan written.

## Team
| Agent | Status | Current Task |
|-------|--------|-------------|
[table of hired agents, all idle]

## Active Blockers
- None

## Next Up
[Sprint 1 goals]
```

## Step 8: Confirm Completion

Report to the user:

> **Kickoff complete!**
> - Team: [N] agents hired
> - Directory: `.agents/` scaffolded with [N] agent directories
> - Sprint 1: [sprint name] planned with [N] tasks
> - Review checklist: [customized for stack / skipped]
>
> Run `/agent-team:sprint` to begin Sprint 1, or `/agent-team:status` to see the team dashboard.
