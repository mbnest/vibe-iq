# Signal Agent

**Role:** Opportunity Identification · [pipeline order](README.md) · [master diagram](../architecture/process-flow.md)

## Purpose

Continuously scan available signal sources to surface candidate opportunities — problems, requests, or ideas worth building — for a human to evaluate. This is detection, not decision: the agent's job ends at "here's a pattern worth looking at," never at "we should build this."

## Inputs

- Support ticket / customer feedback data
- Product usage and telemetry data
- Competitive and market sources
- Internal stakeholder input
- Technical debt and risk registers

## Outputs

- A candidate-opportunity report per finding: what was observed, supporting evidence, rough signal strength, and source.

## Functions

- Market/competitive signal scanning
- Customer feedback and support-ticket mining
- Usage, funnel, and cost-anomaly analysis
- Technical debt and risk surfacing
- Idea capture into a shared backlog

## Autonomy Tiering

| Quadrant | What it looks like here | Behavior |
|---|---|---|
| Known + Safe | Routine scans of standard sources using established detection heuristics (e.g., recurring ticket themes, known competitor set). | Full autonomy — scan and publish the candidate-opportunity report directly. |
| Known + Risky | Signal touches a sensitive data source (e.g., customer complaints containing PII). | Autonomous scan, but the report is redacted/reviewed before wide circulation. |
| Unknown + Safe | Exploring a brand-new signal source (e.g., a new listening tool) in a read-only, non-distributed way. | Autonomous exploration; findings audited before becoming a standing source. |
| Unknown + Risky | Signal suggests legal, safety, or reputational exposure. | Manual — escalate directly, no autonomous framing or publishing. |

## Escalation Triggers

- Signal implies legal, safety, or regulatory exposure
- Signal is derived from identifiable customer data
- Signal contradicts a recent, explicit business decision

## Handoffs

- **Receives from:** no upstream pipeline step — sourced from the external world (customers, market, internal systems).
- **Delivers to:** [Scoping Agent](scoping-agent.md), which evaluates whether and how to act on the opportunity.
