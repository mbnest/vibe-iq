# Decision Log

One line per decision. `D-NN` are the platform-level decisions (numbering inherited from the original [`../notes/platform-notes.md`](../notes/platform-notes.md) journal); `D-PD-NN` are the product-definition phase (full Q&A in [`../notes/product-definition-phase-open-questions.md`](../notes/product-definition-phase-open-questions.md)). Heavyweight decisions also have an [ADR](../adr/). What's *not* yet decided: [`open-questions.md`](open-questions.md).

## Platform

| ID | Decision | Detail |
|---|---|---|
| D-01 | Orchestration is deterministic, durable, distributed; agentic reasoning lives *inside* steps, never in control flow | [runtime.md](../architecture/runtime.md) |
| D-02 | Phase handoffs are durable, versioned artifacts — not implicit conversational context | [data-model.md](../architecture/data-model.md) |
| D-03 | Test agent derives tests from the spec, independent of the diff; Review works from spec + diff with different context | [test](../agents/test-agent.md), [review](../agents/review-agent.md) |
| D-04 | Provisioning, testing infra, data quality, CI/CD are consumed via sibling enablers, not rebuilt — where they exist | [process-flow.md](../architecture/process-flow.md) |
| D-05 | Human gates sit after the spec and before merge/release; middle phases run with less oversight | [process-flow.md](../architecture/process-flow.md) |
| D-06 | Realistic v1 "done" = "PR ready to merge + enabler tickets filed with full context", not autonomous deploy | [process-flow.md](../architecture/process-flow.md) |
| D-07 | Intake starts *after* the product function has scoped and prioritized; it resolves engineering ambiguity, not business priority | [intake](../agents/intake-agent.md) |
| D-08 | Autonomy is assigned per task-graph node, not per pipeline step | [autonomy-tiering.md](../policies/autonomy-tiering.md) |
| D-09 | Infra provisioning bifurcates: catalog path (agentic, policy-checked per parameter set) vs. novel path (ticket) | [enablement](../agents/enablement-agent.md) |
| D-10 | The human gates stay human by design, not by capability gap — agents prep, never clear | [agents/README.md](../agents/README.md) |
| D-11 | `spec.md` is versioned; changes classified by blast radius to set the pipeline re-entry point | [requirement-change-handling.md](../policies/requirement-change-handling.md) |
| D-12 | One architecture diagram, rooted in the business process; every external dependency flagged individually as unconfirmed | [process-flow.md](../architecture/process-flow.md) |
| D-13 | Requirements exist at four precision levels (L1 business / L2 product / L3 architect / L4 senior dev); each requirement tracks its level | [front-of-flow.md](../architecture/front-of-flow.md) |
| D-14 | The front of the flow produces a chain of derived, individually signed artifacts — brief → product definition → solution spec → story specs | [front-of-flow.md](../architecture/front-of-flow.md), [data-model.md](../architecture/data-model.md) |
| D-15 | Work is Initiative → Feature → Story; a high-level solution is signed off before story breakdown (third human gate); the pipeline fans out | [front-of-flow.md](../architecture/front-of-flow.md), [solution-signoff](../agents/solution-signoff-gate.md) |
| D-16 | Brownfield context splits into code and infra; infra MVP cascade is repo Terraform state → cloud-API query, CMDB parked | [process-flow.md](../architecture/process-flow.md) |
| D-17 | The L1→L2 hop is a named agent (the Definition Agent); prioritization stays human ("agent drafts, human decides") | [definition-agent.md](../agents/definition-agent.md) |

## Product-definition phase (2026-09-05)

| ID | Decision | Detail |
|---|---|---|
| D-PD-01 | Product definition is stored as structured requirement records; prose is a generated view | [definition-agent.md](../agents/definition-agent.md), [data-model.md](../architecture/data-model.md) |
| D-PD-02 | Can freeze carrying `[unresolved]` on minor scope forks; a load-bearing fork blocks the freeze | [definition-agent.md](../agents/definition-agent.md) |
| D-PD-03 | Metric *definitions* must be present and target *values* filled (by agent or person) before the artifact moves forward | [definition-agent.md](../agents/definition-agent.md) |
| D-PD-04 | The Definition Agent drafts the feature breakout; product curates; it is part of the frozen artifact | [definition-agent.md](../agents/definition-agent.md) |
| D-PD-05 | Every requirement is level-tagged, but tags *inform* the layer owner — they do not *bind* | [data-model.md](../architecture/data-model.md) |
| D-PD-06 | Autonomy: agent publishes mirror-back, scope-forks, risks, NFR draft, candidate personas/metrics/features; flags the rest | [definition-agent.md](../agents/definition-agent.md) |
| D-PD-07 | In the in-review loop, product can decline a stakeholder edit with rationale — product owns the artifact | [definition-agent.md](../agents/definition-agent.md) |
| D-PD-08 | The chatbot sidekick may write to form fields directly; the user confirms on submit | [interface note](../architecture/runtime.md), [project-twin](../notes/project-twin.md) |
| D-PD-09 | A thin request fills what it can from context, then bounces only irreducible gaps to the stakeholder | [definition-agent.md](../agents/definition-agent.md) |
| D-PD-10 | An agent completeness check against a fixed checklist gates product commit (override logged) | [definition-agent.md](../agents/definition-agent.md) |
| D-PD-11 | All affected stakeholders must sign; the tool must always show who has not yet approved | [definition-agent.md](../agents/definition-agent.md) |
| D-PD-12 | The stakeholder sign-off attests to mirror-back fidelity only; product-introduced content is product-attested | [definition-agent.md](../agents/definition-agent.md) |
| D-PD-13 | Provenance tags reuse the Intake vocabulary with a documented rebinding for no-live-sponsor cases | [data-model.md](../architecture/data-model.md) |
| D-PD-14 | Product-definition sign-off creates the change_request record; solution sign-off decides cheap vs. expensive processing | [data-model.md](../architecture/data-model.md), [solution-signoff](../agents/solution-signoff-gate.md) |

## ADRs

| ADR | Title | Status |
|---|---|---|
| [0001](../adr/0001-record-architecture-decisions.md) | Record architecture decisions | Accepted |
| [0002](../adr/0002-target-application-stack.md) | Target application stack for the MVP | Accepted |
| [0003](../adr/0003-allow-sqlite-for-local-and-low-volume-workloads.md) | Allow SQLite for local and low-volume workloads | Accepted |
| 0004 | Orchestration substrate | Flagged, not written — revisit when front-of-flow build starts |
| 0005 | MVP hosting | Flagged, not written — revisit when always-on multi-user access is needed |
| 0006 | Model routing policy | Flagged, not written — revisit when eval data exists |

## Still open

The full index is **[`open-questions.md`](open-questions.md)**. The top blocker: the `spec.md` schema is undecided.
