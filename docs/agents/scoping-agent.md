# Scoping Agent

**Role:** Prioritization & Scoping · [pipeline order](README.md) · [master diagram](../architecture/process-flow.md)

## Purpose

Turn a candidate opportunity or direct stakeholder ask into a scoped, prioritized ask ready for engineering intake. The agent drafts the decision inputs; the prioritization decision itself remains a business call owned by the vertical's product function.

## Inputs

- Opportunity report (from the Signal Agent) or a direct stakeholder request
- Current roadmap and OKRs
- Budget and staffing constraints
- Cross-team dependency map

## Outputs

- Scoped-ask summary: effort/ROI estimate, dependency map, roadmap-fit assessment.

## Functions

- Business case / ROI estimation drafting
- Effort sizing (t-shirt sizing, rough estimation)
- Roadmap placement and sequencing analysis
- Cross-team dependency mapping
- OKR/goal alignment check
- Budget and staffing allocation drafting

## Autonomy Tiering

| Quadrant | What it looks like here | Behavior |
|---|---|---|
| Known + Safe | Sizing and roadmap-fit analysis using established estimation models and existing roadmap data. | Full autonomy — draft and publish the scoped-ask summary directly. |
| Known + Risky | The ask would reprioritize other teams' committed work (cross-team resourcing conflict). | Autonomous drafting of the tradeoff analysis; the reprioritization decision itself is human-gated. |
| Unknown + Safe | A small, isolated, low-cost experiment with no roadmap conflict. | Autonomous drafting and provisional scoping. |
| Unknown + Risky | Budget, legal, or compliance impact is unclear. | Manual — human scopes it. |

## Escalation Triggers

- Ask would reprioritize another team's committed work
- Budget impact exceeds a defined threshold
- Conflicting stakeholder priorities surface during scoping

## Handoffs

- **Receives from:** [Signal Agent](signal-agent.md), or a direct stakeholder ask entering the process independently.
- **Delivers to:** in the v0.5 front-of-flow model ([`front-of-flow.md`](../architecture/front-of-flow.md)), the [Definition Agent](definition-agent.md) — which translates the prioritized ask from L1 to L2 and produces the product definition — once the product function has made the prioritization decision. Per-story [Intake](intake-agent.md) then runs downstream of solution sign-off, not directly off Scoping. (The master diagram still shows the older Scoping → Intake edge; the front-of-flow expansion is pending integration there.)

## Relationship to the Definition Agent

Scoping and Definition both touch product-shaped work and the boundary is worth stating: Scoping drafts the **decision inputs for prioritization** (effort/ROI, roadmap fit, dependencies) and stops at the prioritization decision. The [Definition Agent](definition-agent.md) picks up an *already-prioritized* ask and does the **L1→L2 translation** (mirror-back plus product-introduced requirements, NFRs, targets, feature breakout). Where they appear to overlap — ROI drafting, OKR alignment — Scoping's version is a rough sizing input to the prioritization call; Definition's is the committed success framing inside a definition that has been prioritized.
