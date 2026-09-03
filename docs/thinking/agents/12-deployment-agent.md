# Deployment Agent

**Pipeline step:** 12. Deployment / Go-Live · [← back to master](../sdlc-business-process-flow.md)

## Purpose

Ship the approved, provisioned change to production.

## Inputs

- Approved diff
- Provisioned infrastructure (from the Enablement Agent)

## Outputs

- Live deployment, deployment record

## Functions

- Deployment scheduling / change windows
- Canary, blue-green, or rolling deployment execution
- Feature-flag rollout
- Post-deploy smoke testing
- Rollback plan preparation and execution
- Stakeholder/user communication
- Release notes publication

*Note:* much of this step is already deterministic CI/CD rather than agentic reasoning. The agent's real value-add is judgment calls — like when to roll back — not the mechanical act of deploying.

## Autonomy Tiering

| Quadrant | What it looks like here | Behavior |
|---|---|---|
| Known + Safe | Standard rollout of a known-safe change. | Full autonomy — execute the rollout directly. |
| Known + Risky | Standard rollout of a known-risky change. | Autonomous execution; rollback decision is human-gated. |
| Unknown + Safe | Novel change, feature-flagged and isolated. | Autonomous rollout; audited after, not gated before. |
| Unknown + Risky | Novel change with unclear blast radius. | Manual — human-led rollout. |

## Escalation Triggers

- Smoke test failure post-deploy
- Error-rate or latency threshold breach post-deploy
- Rollback is needed on a Known+Risky or Unknown change

## Handoffs

- **Receives from:** [Enablement Agent](11-enablement-agent.md) (step 11).
- **Delivers to:** [Validation Agent](13-validation-agent.md) (step 13).
