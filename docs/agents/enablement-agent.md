# Enablement Agent

**Role:** Provisioning & Enablement · [pipeline order](README.md) · [master diagram](../architecture/process-flow.md)

## Purpose

Get the infrastructure, environments, and pipeline changes a release needs — favoring self-service from a pre-approved catalog wherever the request matches something already vetted, and falling back to the existing ticket-based, human-fulfilled enablers only for what hasn't been vetted.

## Inputs

- Approved change (from the Merge & Release Approval gate)
- Infra requirements identified during Design/Planning

## Outputs

- Provisioned/modified infrastructure (catalog path), or a filed ticket with full context (novel path)

## The Two Paths

This is the one step in the pipeline that bifurcates by design, not just by risk tier:

- **Catalog path (agentic).** The ask maps to Terraform (or equivalent) modules that have already passed compliance and security review and live in git as approved building blocks. The agent selects the module, supplies parameters, and applies it directly — subject to policy-as-code checks (OPA/Sentinel/Conftest-style) evaluated against the *specific parameter set*, not just the module's identity. A module passing review once doesn't make every future parameterization safe: an approved S3-bucket module can still be instantiated with public-read, an approved compute module can still be sized into a cost or blast-radius problem.
- **Novel path (ticket/human-fulfilled).** The ask needs a new module, a variance from an approved pattern, or crosses into something the existing review never covered. Routed to the enabler team via ticket, exactly per the original ticket-based assumption in the source notes.

**Dependency-aware execution:** a single request can be decomposed at the task-graph level. Everything whose dependency closure touches only catalog (known-safe) modules proceeds agentically; only the genuinely novel node — and anything downstream of it — routes to the novel path. The whole request is never gated on its hardest 10%.

## Functions

- Catalog module selection and parameterization
- Policy-as-code validation of the specific instantiation
- Ticket filing with full context for novel requests
- Status tracking/polling on filed tickets
- Task-graph decomposition into catalog-eligible vs. novel nodes

## Autonomy Tiering

| Quadrant | What it looks like here | Behavior |
|---|---|---|
| Known + Safe | Catalog module, standard parameters. | Full autonomy — select, parameterize, apply directly. |
| Known + Risky | Catalog module, high-blast-radius parameters (prod, public exposure). | Autonomous prep and policy check; human-gated apply. |
| Unknown + Safe | Net-new module, sandboxed/dev-only. | Autonomous execution; audited after, not gated before. |
| Unknown + Risky | Net-new module touching prod, shared, or regulated resources. | Manual — routed as a ticket to the enabler team. |

## Escalation Triggers

- Parameter set fails a policy-as-code check, even on an approved catalog module
- Request touches a regulated data classification
- Catalog module hasn't been re-validated within its governance window (a curation concern, not a per-request one)

## Handoffs

- **Receives from:** [Merge & Release Approval Gate](merge-release-approval-gate.md).
- **Delivers to:** [Deployment Agent](deployment-agent.md) once infra is ready; or to the human-fulfilled enabler queue for novel requests, with an async signal back to the pipeline on completion.
