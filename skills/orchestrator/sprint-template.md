# Sprint Plan Template

> Copy this template to `.agents/orchestrator/sprint-plan.md` and fill in for each sprint.

---

```markdown
# Sprint [N] — [Sprint Name]
**Dates**: [YYYY-MM-DD] → [YYYY-MM-DD]
**Goal**: [1-2 sentence sprint goal]
**Autonomy Level**: [Low / Medium / High]

---

## Tasks

| ID | Agent | Task | Status | Blocked By |
|----|-------|------|--------|------------|
| S[N]-D01 | Design | [task] | pending | — |
| S[N]-C01 | Coding | [task] | pending | S[N]-D01 |
| S[N]-CX01 | Codex | Review [feature] | pending | S[N]-C01 |
| S[N]-DP01 | Deploy | [task] | pending | — |
| S[N]-CN01 | Content | [task] | pending | — |
| S[N]-G01 | Growth | [task] | pending | — |
| S[N]-QA01 | QA | [task] | pending | — |

## Agent Parallelization

### Parallel (no dependencies)
```
Content ── [task description]
Growth ─── [task description]
QA ─────── [task description]
Deploy ─── [task description]
```

### Sequential (dependency chain)
```
Design → Coding → Codex → Coding (fixes) → Deploy
```

## Acceptance Criteria
- [ ] [Criterion 1]
- [ ] [Criterion 2]
- [ ] [Criterion 3]
```

---

## Task ID Convention

Format: `S[sprint]-[agent code][number]`

| Agent | Code | Example |
|-------|------|---------|
| Design | D | S1-D01 |
| Coding | C | S1-C01 |
| Codex | CX | S1-CX01 |
| Deploy | DP | S1-DP01 |
| Content | CN | S1-CN01 |
| Growth | G | S1-G01 |
| QA | QA | S1-QA01 |

## Status Values

| Status | Meaning |
|--------|---------|
| `pending` | Not started |
| `in_progress` | Being worked on |
| `blocked` | Waiting on dependency |
| `done` | Completed |

## Recommended Sprint Cadence

| Sprint | Focus | Typical Active Agents |
|--------|-------|----------------------|
| 1 | Foundation & Design | Design, Deploy (scaffold), Content, Growth, QA (test plans) |
| 2 | Core Build | Coding, Codex, Deploy, Content, Growth, QA |
| 3-4 | Features | All agents |
| 5-6 | Polish & Content | All agents, Content/QA heaviest |
| 7-8 | Launch | Deploy, Growth, QA heaviest |
