# Handoff Templates

> Templates for inter-agent handoff documents. During kickoff, these are created in `.agents/shared/handoffs/`.

---

## 1. Design → Coding Handoff

File: `.agents/shared/handoffs/design-to-coding.md`

```markdown
# Design → Coding Handoff

**Sprint**: [N] → [N+1]
**Date**: [YYYY-MM-DD]
**Status**: Ready for development

---

## Design Source

[Reference to Figma file, design tool, or spec document]
- **File/URL**: [link or file key]
- **Access**: [how to read the designs]

---

## Design System Tokens

### Colors
[Color tokens as code constants — primary, accent, neutral, semantic]

### Typography
[Font families, text styles with size/weight/line-height]

### Spacing & Layout
[Spacing scale, border radius tokens, layout constants]

### Shadows
[Shadow definitions]

### Icons
[Icon library, default size, stroke weight, key icons]

---

## Screens Ready for Development

### [Screen Name]
| Section | Key Components | Notes |
|---------|---------------|-------|
| [section] | [components] | [implementation notes] |

---

## Code-Phase Fixes

Issues noted during design review to address during implementation:

| # | Screen | Fix Required | Priority |
|---|--------|-------------|----------|
| 1 | [screen] | [fix description] | High/Medium/Low |

---

## Handoff Checklist
- [ ] All screens reviewed and approved
- [ ] Design tokens extracted
- [ ] Component hierarchy documented per screen
- [ ] Edge cases and states identified
- [ ] Assets exported or asset strategy defined
```

---

## 2. Coding → Codex Review Handoff

File: `.agents/shared/handoffs/coding-to-codex.md`

```markdown
# Coding → Codex Review Handoff

**Protocol**: Every code deliverable from the Coding Agent goes through Codex review before being marked as done.

---

## Workflow

Coding Agent → adds to Review Queue → Codex Agent reviews → moves to Feedback Ready → Coding Agent addresses → moves to Done

## Review Queue
_Coding Agent adds items here when code is ready for review._

| # | Files / Feature | Coding Task ID | Date Submitted | Priority |
|---|----------------|----------------|----------------|----------|

## Feedback Ready
_Codex Agent moves items here after writing review._

| # | Files / Feature | Review File | Verdict | Date Reviewed |
|---|----------------|-------------|---------|---------------|

## Done
_Coding Agent moves items here after addressing feedback._

| # | Files / Feature | Review File | Final Verdict | Date Closed |
|---|----------------|-------------|---------------|-------------|
```

---

## 3. Coding → Deploy Handoff

File: `.agents/shared/handoffs/coding-to-deploy.md`

```markdown
# Coding → Deploy Handoff

## Ready for Deployment

| Build | Version | Status | Notes |
|-------|---------|--------|-------|

## New Environment Variables

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|

## Database Migrations

| Migration | Description | Reversible | Notes |
|-----------|------------|------------|-------|

## Deploy Checklist
- [ ] All tests passing
- [ ] No TypeScript / lint errors
- [ ] Environment variables documented
- [ ] Database migrations ready
- [ ] Assets optimized
- [ ] Version bumped
```
