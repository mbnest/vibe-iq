# Design Agent

**Role:** Design · [pipeline order](README.md) · [master diagram](../architecture/process-flow.md)

## Purpose

Turn an approved spec into a technical design ready for planning.

## Inputs

- `spec.md`
- Existing infra inventory, where the ask touches infrastructure — repo Terraform state → cloud-API cascade (see External Dependency Status in [`process-flow.md`](../architecture/process-flow.md)); needed to decide new-vs-extend rather than designing blind to what's already provisioned

## Outputs

- `design.md` — architecture, API/schema design, documented tradeoffs (ADRs).

## Functions

- System/architecture design
- API and interface design
- Data schema design
- Threat modeling / security design pass
- Scalability and performance design
- Tradeoff documentation (ADRs)
- UX/UI design, where applicable

## Autonomy Tiering

| Quadrant | What it looks like here | Behavior |
|---|---|---|
| Known + Safe | Extending an established architectural pattern. | Full autonomy — draft `design.md` directly. |
| Known + Risky | A new service touching shared or critical infrastructure. | Autonomous drafting, human-gated approval before it drives planning. |
| Unknown + Safe | An isolated, experimental component with no shared dependents. | Autonomous drafting; reviewed after, not gated before. |
| Unknown + Risky | Novel architecture with unclear failure modes. | Manual — human designs it, agent assists with research and drafting only. |

## Escalation Triggers

- Design implies new external exposure or a new data classification
- Failure modes of the proposed architecture are not well understood
- Design touches infrastructure shared by other teams/verticals
- Existing infra state can't be determined (no repo IaC, no reachable cloud-API answer) — the new-vs-extend call can't be made autonomously and routes to a human infra owner

## Handoffs

- **Receives from:** [Requirements Sign-off Gate](requirements-signoff-gate.md).
- **Delivers to:** [Planning Agent](planning-agent.md).
