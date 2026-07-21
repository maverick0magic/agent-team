---
name: family-office
description: Stand up a family office to optimize after-tax, after-fee, risk-adjusted returns. Sizes the structure to assets, fills the seven core functions, and produces a mandate, IPS, and strategic asset allocation. Educational only, not financial/tax/legal advice.
arguments:
  - name: phase
    description: "Optional phase to jump to: discovery | structure | governance | allocate | operate. Auto-detects if omitted."
    required: false
user-invocable: true
---

# Command: family-office

## When to Use
Run this to design or run a family office — a single-family office (SFO), a multi-family
/ outsourced office (MFO/OCIO), or a lean virtual office. It optimizes for **after-tax,
after-fee, risk-adjusted returns compounded across generations**, not headline returns.

> ⚠️ Educational framework, not financial, tax, or legal advice. Ratify with licensed
> tax counsel, an estate attorney, and an investment fiduciary in the relevant jurisdiction.

## Steps
1. Adopt the **Hedge Fund Manager / CIO** role (`agents/hedge-fund-manager-agent.md`).
2. Load the **Family Office Setup** skill (`skills/family-office/SKILL.md`) and follow its
   phased protocol:
   - **Discovery** — assets, objective, jurisdiction, liquidity needs, constraints.
   - **Structure** — size the model to assets (virtual < ~$30M · MFO/OCIO ~$30–100M ·
     SFO ~$100M+); pick entities, trusts, and custody with counsel.
   - **Governance** — write the IPS from `skills/family-office/investment-policy-statement.md`.
   - **Allocate** — pick a policy portfolio from `skills/family-office/asset-allocation-models.md`.
   - **Operate** — quarterly investment committee, rebalancing, tax-loss harvesting, fee
     audit, risk monitoring, and periodic performance/investor reporting.
3. If `{{args.phase}}` is provided, jump to that phase; otherwise auto-detect the next
   phase from what already exists in `.agents/hedge-fund/`.
4. Write all deliverables under `.agents/hedge-fund/` (discovery, mandate, functions-map,
   ips, structure, portfolio, risk-report, performance).
5. Surface assumptions, risks, and what would change the recommendation. Recommend
   licensed professionals before any real action.
