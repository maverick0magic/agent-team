# Agent Team

A Claude Code plugin that gives you a 7-agent team for building software products. Battle-tested across multiple production projects.

## What It Does

Agent Team turns Claude Code into a **team of specialized agents** that work together through structured sprints, code reviews, and inter-agent handoffs. Instead of one AI assistant doing everything, you get focused agents that each own a domain.

```
Orchestrator (you + Claude Code)
  ├── Design Agent ────────── UI/UX, design system, visual QA
  ├── Coding Agent ──────────  implementation, all code
  │       ↓
  ├── Codex Agent ─────────── code review (8-point checklist)
  │       ↓
  ├── Deploy Agent ────────── CI/CD, infra, releases
  ├── Content Agent ───────── copy, content files, data
  ├── Growth Agent ────────── strategy, ASO, launch
  └── QA Agent ────────────── test plans, testing, a11y
```

## Quick Start

### Install

```bash
# Clone the plugin
git clone https://github.com/ankitgovil/agent-team.git

# Use it in any project
cd your-project
claude --plugin-dir /path/to/agent-team
```

### First Run

```
/agent-team:kickoff
```

This will:
1. Ask if it's a personal or work project
2. Scan your codebase to detect the tech stack
3. Propose a team (2-4 agents for personal, 5-7 for work)
4. Scaffold the `.agents/` directory after you approve
5. Write Sprint 1 plan
6. Generate a stack-specific code review checklist

### Day-to-Day

```
/agent-team:sprint           # Auto-detects next phase and runs it
/agent-team:sprint plan      # Plan the next sprint
/agent-team:sprint dispatch  # Spawn agents to work in parallel
/agent-team:sprint review    # Review agent outputs, trigger code reviews
/agent-team:sprint report    # Sprint summary and retrospective

/agent-team:review           # Code review on staged/recent changes
/agent-team:review src/      # Code review on specific files

/agent-team:status           # Team dashboard (all agents)
/agent-team:status coding    # Detailed view of one agent
```

## Commands

| Command | Description |
|---------|-------------|
| `/agent-team:kickoff` | Initialize team for a project. Classifies, assesses, proposes team, scaffolds. |
| `/agent-team:sprint` | Sprint lifecycle manager. Plan → Dispatch → Review → Report cycle. |
| `/agent-team:review` | Trigger Codex code review on files or changes. |
| `/agent-team:status` | Read-only team dashboard with agent statuses and task counts. |

## The 7 Agents

| Agent | Specialty | When to Hire | Model |
|-------|-----------|-------------|-------|
| **Design** | UI/UX, design tokens, Figma workflows, visual QA | Projects with a frontend | inherit |
| **Coding** | Full-stack implementation, all application code | Always | inherit |
| **Codex** | Code review gate, 8-point quality checklist | Work projects (mandatory); optional for personal | inherit |
| **Deploy** | CI/CD, environments, releases, infrastructure | Projects that ship to production | sonnet |
| **Content** | App copy, content files, marketing content | Content-heavy projects | sonnet |
| **Growth** | Acquisition, ASO, monetization, launch strategy | Products with users | sonnet |
| **QA** | Test plans, testing, accessibility, performance | Work projects; recommended for personal | sonnet |

### Personal vs Work

| | Personal | Work |
|---|---------|------|
| Team size | 2-4 agents | 5-7 agents |
| Codex reviews | Optional | Mandatory |
| Default agents | Coding + Deploy | Coding, Design, Codex, Deploy, QA |
| Autonomy | Medium-High | Medium |

## Orchestrator Protocol

The main Claude Code session acts as the **Orchestrator**, following a repeating 4-phase cycle:

```
┌─────────┐     ┌──────────┐     ┌─────────┐     ┌─────────┐
│  PLAN   │ ──► │ DISPATCH │ ──► │ REVIEW  │ ──► │ REPORT  │
│         │     │          │     │         │     │         │
│ Read    │     │ Spawn    │     │ Read    │     │ Summary │
│ state,  │     │ agents   │     │ outputs,│     │ retro,  │
│ propose │     │ in       │     │ trigger │     │ next    │
│ tasks   │     │ parallel │     │ Codex   │     │ sprint  │
└─────────┘     └──────────┘     └─────────┘     └─────────┘
```

## Codex 8-Point Review

Every code deliverable is reviewed against 8 categories, customized for your stack during kickoff:

1. **Design Compliance** — tokens, colors, typography, spacing
2. **Type Safety** — no `any`, proper interfaces, shared types
3. **Framework Practices** — idiomatic patterns for your stack
4. **State Management** — focused stores, loading/error states
5. **Security** — no secrets, auth, validation, OWASP top 10
6. **Performance** — efficient rendering, optimized assets
7. **Accessibility** — labels, roles, contrast, screen readers
8. **Structure** — <300 line files, naming conventions, no dead code

Verdicts: **APPROVE** | **APPROVE WITH COMMENTS** | **REQUEST CHANGES** | **REJECT**

## Agent Handoff Chain

Agents communicate through structured handoff documents:

```
Design ──► design-to-coding.md ──► Coding ──► coding-to-codex.md ──► Codex
                                                                        │
Coding ◄── feedback via codex reviews ◄─────────────────────────────────┘
   │
   ▼
coding-to-deploy.md ──► Deploy ──► Release
```

## Directory Structure

After kickoff, your project gets an `.agents/` directory:

```
.agents/
├── orchestrator/           # Sprint plans, project status, decisions
│   ├── sprint-plan.md
│   ├── project-status.md
│   └── decision-log.md
├── design/                 # Design tasks, Figma inventory, reviews
│   ├── status.md
│   └── tasks.md
├── coding/                 # Code tasks, tech decisions
│   ├── status.md
│   └── tasks.md
├── codex/                  # Review checklist, code reviews
│   ├── status.md
│   ├── tasks.md
│   ├── review-checklist.md
│   └── reviews/
├── deploy/                 # Environments, release checklists
│   ├── status.md
│   └── tasks.md
├── content/                # Content tasks
│   ├── status.md
│   └── tasks.md
├── growth/                 # Strategy docs, marketing
│   ├── status.md
│   └── tasks.md
├── qa/                     # Test plans, bug reports
│   ├── status.md
│   └── tasks.md
└── shared/
    └── handoffs/           # Inter-agent handoff documents
        ├── design-to-coding.md
        ├── coding-to-codex.md
        └── coding-to-deploy.md
```

## Requirements

- [Claude Code CLI](https://docs.anthropic.com/en/docs/claude-code)
- A git repository (for code review diffs)

## Recommended Sprint Cadence

| Sprint | Focus | Key Agents |
|--------|-------|------------|
| 1 | Foundation & Design | Design, Deploy (scaffold), Content, Growth, QA (plans) |
| 2 | Core Build | Coding, Codex, Deploy |
| 3-4 | Features | All agents |
| 5-6 | Polish | All agents, Content/QA heaviest |
| 7-8 | Launch | Deploy, Growth, QA heaviest |

## Autonomy Levels

| Level | Behavior | When to Use |
|-------|----------|-------------|
| Low | Ask before every action | New projects, unfamiliar codebases |
| Medium | Propose → approve → execute | Default for most projects |
| High | Execute autonomously, report results | Trusted, well-defined tasks |

## License

MIT
