# Validation Agent

**Role:** Post-Release Validation & Monitoring · [pipeline order](README.md) · [master diagram](../architecture/process-flow.md)

## Purpose

Confirm the change behaves as intended in production, against the success criteria defined back in the spec — closing the loop between what was asked for and what actually shipped.

## Inputs

- Deployment record
- `spec.md` (success criteria)
- Live metrics/logs

## Outputs

- Validation report: pass/fail against defined success criteria

## Functions

- Metrics/dashboard monitoring
- Alert threshold configuration and watching
- Log aggregation review
- Correlating the deploy against metric shifts
- A/B or success-metric validation
- SLO/SLA compliance checks

## Autonomy Tiering

| Quadrant | What it looks like here | Behavior |
|---|---|---|
| Known + Safe | Monitoring standard dashboards/alerts against known baselines and established success criteria. | Full autonomy — monitor and report directly. |
| Known + Risky | Validating a deploy to a high-traffic or critical system. | Autonomous monitoring and reporting; any rollback decision routes to a human or a defined runbook. |
| Unknown + Safe | Validating a low-stakes experimental feature with no established baseline yet. | Autonomous — agent defines ad hoc thresholds and acts within a bounded blast radius. |
| Unknown + Risky | Anomaly with unclear cause on a critical system. | Manual — escalate as an incident immediately, no autonomous remediation. |

## Escalation Triggers

- Anomaly on a critical system with unclear root cause
- Success criteria not met
- Situation crosses from "validation" into an actual incident

## Handoffs

- **Receives from:** [Deployment Agent](deployment-agent.md).
- **Delivers to:** Step 14, Incident Response (out of current scope) when validation surfaces an incident; otherwise closes the loop back into step 15, Maintenance & Iteration, once validation passes cleanly.
