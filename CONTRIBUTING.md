# Contributing to Agent Team

Thanks for your interest in Agent Team! This project is designed to be radically easy to contribute to. Every agent, command, and workflow is defined in Markdown — no build step, no runtime, no framework.

## Quick Start

1. Fork the repo
2. Clone your fork
3. Make your changes
4. Test with Claude Code (install the plugin locally)
5. Submit a PR

## What You Can Contribute

### Add a New Agent

Adding an agent is writing a single Markdown file. Here's how:

1. Create a new file in `agents/` (e.g., `agents/security-agent.md`)
2. Use this template:

```markdown
---
name: Security
description: Security auditing, vulnerability scanning, dependency checks, and OWASP compliance
tools:
  - Read
  - Glob
  - Grep
  - Bash
model: sonnet
color: red
---

# Security Agent

## Role
[Describe the agent's responsibility in 2-3 sentences]

## Scope
- [What this agent owns]
- [What it does NOT own]

## Workflow
1. [Step-by-step process the agent follows]

## Output Format
[What the agent produces — reports, files, etc.]

## Handoffs
- **Receives from:** [Which agents hand off to this one]
- **Hands off to:** [Which agents this one feeds into]
```

3. Choose tools carefully:
   - `Read`, `Glob`, `Grep` — safe for any agent
   - `Write` — for agents that create files/reports
   - `Edit` — for agents that modify existing code
   - `Bash` — for agents that need to run commands (tests, builds, deploys)
   - `WebSearch` — for agents that need to research external information
4. Choose a model:
   - `inherit` — uses the parent session model (for high-complexity work)
   - `sonnet` — faster/cheaper (for procedural or less critical work)

### Agent Ideas We'd Love

| Agent | Description | Difficulty |
|-------|-------------|------------|
| **Security** | OWASP audit, dependency scanning, secret detection | Medium |
| **Analytics** | Tracking setup, event schemas, dashboard configs | Medium |
| **i18n** | Internationalization, string extraction, locale management | Easy |
| **Docs** | API docs, user guides, changelog generation | Easy |
| **Database** | Schema design, migrations, query optimization | Medium |
| **API** | API design, OpenAPI specs, endpoint scaffolding | Medium |
| **Performance** | Lighthouse audits, bundle analysis, optimization | Medium |
| **Accessibility** | Dedicated a11y audits beyond what QA covers | Easy |

### Add a New Command

Commands live in `commands/` as Markdown files with YAML frontmatter:

```markdown
---
name: your-command
description: What this command does
arguments:
  - name: arg1
    description: Description of the argument
    required: false
---

# Command: your-command

## When to Use
[When should the user run this command]

## Steps
1. [What the command does, step by step]
```

### Improve Existing Agents

- Refine agent instructions for better output quality
- Add edge case handling
- Improve handoff templates
- Add stack-specific customizations to the Codex checklist

### Improve Documentation

- Fix typos or unclear instructions in the README
- Add examples and use cases
- Write tutorials

## Guidelines

- **Keep it Markdown.** No JavaScript, no build tools, no dependencies. The zero-dependency nature is a feature.
- **Respect tool restrictions.** Agent tool access is a deliberate design decision. Codex can't edit code on purpose. Don't give agents more tools than they need.
- **Test your changes.** Install the plugin locally and run through a workflow with your changes.
- **Keep files focused.** Each agent/command should do one thing well.
- **Write for humans.** Agent definitions are read by AI, but they should be readable by humans too.

## PR Process

1. Create a branch from `main`
2. Make your changes
3. Write a clear PR description explaining what you added and why
4. If adding a new agent, include a brief demo of it in action
5. Submit — we'll review and provide feedback

## Code of Conduct

Be kind. Be constructive. We're here to make building joyful again.

## Questions?

Open a GitHub Discussion or issue. We're happy to help you get started.
