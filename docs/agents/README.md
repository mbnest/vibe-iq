# Agents & Gates

One spec per agent or human gate. Each is a candidate feature / development effort in its own right. Every agent is `inputs → artifact` in isolation — the "what runs next" logic lives in the orchestration layer ([`../architecture/runtime.md`](../architecture/runtime.md)), not in the agents.

## Pipeline order

The pipeline **fans out**: the front-of-flow steps run once per initiative; the build-loop steps run once per story. See [`../architecture/front-of-flow.md`](../architecture/front-of-flow.md) for the fan-out model and [`../architecture/process-flow.md`](../architecture/process-flow.md) for the master diagram.

### Front of the flow — per initiative

| Order | Agent / Gate | Transform | Type |
|---|---|---|---|
| 1 | [Signal Agent](signal-agent.md) | world → candidate opportunity | Agent |
| 2 | [Scoping Agent](scoping-agent.md) | opportunity → scoped, prioritized ask | Agent |
| — | *(prioritization decision)* | — | Human (product function) |
| 3 | [Definition Agent](definition-agent.md) | business brief (L1) → product definition (L2) | Agent |
| 4 | *Solution analysis* | product definition (L2) → solution spec (L3) | Agent |
| 5 | [Solution Sign-off Gate](solution-signoff-gate.md) | freeze the solution before story breakdown | Human Gate |
| 6 | *Feature / story generation* | solution spec → backlog tickets | Agent |

### Build loop — per story

| Order | Agent / Gate | Artifact out | Type |
|---|---|---|---|
| 7 | [Intake Agent](intake-agent.md) | `spec.md` | Agent |
| 8 | [Requirements Sign-off Gate](requirements-signoff-gate.md) | — | Human Gate |
| 9 | [Design Agent](design-agent.md) | `design.md` | Agent |
| 10 | [Planning Agent](planning-agent.md) | `task-graph.json` | Agent |
| 11 | [Implementation Agent(s)](implementation-agent.md) | diff(s) | Agent |
| 12 | [Test Agent](test-agent.md) | test report | Agent |
| 13 | [Review Agent](review-agent.md) | review findings | Agent |
| 14 | [Merge & Release Approval Gate](merge-release-approval-gate.md) | — | Human Gate |
| 15 | [Enablement Agent](enablement-agent.md) | provisioned infra / filed ticket | Agent (catalog / novel path) |
| 16 | [Deployment Agent](deployment-agent.md) | live deployment + record | Agent |
| 17 | [Validation Agent](validation-agent.md) | validation report | Agent |

*The **Order** column is this table's own numbering, not part of the filenames — files are named by role (`intake-agent.md`, `solution-signoff-gate.md`). The order is a reading aid; the real sequence is the fan-out described above, and it is still being designed.*

### Ongoing operations — not yet scoped

Incident Response, Maintenance & Iteration, Deprecation / Sunset. No agent designed yet.

## How to read an agent spec

Each doc follows the same shape:

- **Purpose** — the one transform it owns
- **Inputs / Outputs** — the artifacts in and out
- **Functions** — what it does internally
- **Autonomy Tiering** — how the [Known/Unknown × Safe/Risky](../policies/autonomy-tiering.md) model applies to this step
- **Escalation Triggers** — when it stops and asks for a human
- **Handoffs** — the adjacent steps

## The three human gates

[Requirements Sign-off](requirements-signoff-gate.md), [Solution Sign-off](solution-signoff-gate.md), and [Merge & Release Approval](merge-release-approval-gate.md) stay human by design, not by capability gap ([`../decisions/log.md`](../decisions/log.md), D-10). Agents prepare and summarize for these gates; they do not clear them.
