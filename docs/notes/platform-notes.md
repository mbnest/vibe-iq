# VibeIQ — Original Architecture Notes

**Historical record. Retired as the active index (2026-09-05).** This file was the running journal of the pre-planning conversation (v0.1–v0.8). Its content has been promoted into the structured docs:

| What was here | Now lives in |
|---|---|
| Key decisions (1–16) | [`../decisions/log.md`](../decisions/log.md) (D-01–D-17) |
| Open questions / "where to pick this up" | [`../decisions/open-questions.md`](../decisions/open-questions.md) |
| Unverified assumptions | [`../decisions/open-questions.md`](../decisions/open-questions.md#unverified-assumptions) |
| Core thesis, context | [`../overview.md`](../overview.md) |
| The architecture diagram | [`../architecture/process-flow.md`](../architecture/process-flow.md) |
| Companion-doc descriptions | [`../README.md`](../README.md) |

Kept below: the **core thesis** as originally stated, and the **version history** — the record of how the design got here, which the promoted docs summarize but don't narrate.

---

## Core thesis (as originally written)

The platform's value is orchestration and composition, not reimplementation. It takes an already-prioritized ask from a vertical and runs it through a spec-driven pipeline with real SDLC gates (requirements → design → plan → implement → test → review → release), while delegating anything infra-, testing-, data-quality-, or CI/CD-related to the sibling enablers — existence and maturity per capability not yet confirmed — rather than rebuilding that tooling. The differentiator versus "just run a coding agent well" is that this pipeline encodes the org's golden paths once, centrally, so every vertical inherits them automatically instead of re-learning how to correctly invoke every other platform team's tooling.

The platform comes from a horizontal enablement team serving business verticals that each have some (variable) engineering capacity and their own product function. Several sibling horizontal enablers are understood to exist (cloud build / Terraform, quality & testing, data quality, developer experience) — tracked as unconfirmed, per-capability assumptions, assumed ticket-based and human-fulfilled rather than programmatic.

## Version history

| Version | What it added |
|---|---|
| **v0.1** | The original pre-planning sketch: a single spec → design → plan → implement → test → review → release pipeline. |
| **v0.2** | Generalized into a 16-step business-process lifecycle (Opportunity Identification → Deprecation), with a 13-step build cycle segregated from a 3-step ops tail. An agent per build-cycle step. The autonomy-tiering framework (Known/Unknown × Safe/Risky), applied per task-graph node. Requirement change management: versioned `spec.md`, changes classified by blast radius. |
| **v0.3** | Unified the architecture into one business-process-rooted diagram. Broke the single "existing enablers" assumption into five individually-flagged external dependencies. |
| **v0.4** | Removed the diagram from this file — made the process-flow doc its single home — to kill a drift risk. |
| **v0.5** | Added the front of the flow: the L1–L4 requirement-levels model, the derived artifact chain (brief → product definition → solution spec → story specs), the initiative → feature → story hierarchy, and a solution sign-off gate before story breakdown. |
| **v0.6** | Separated two "rules" concerns — pipeline governance vs. business rules inside the built product — and placed a decision engine relative to each. |
| **v0.7** | Added the runtime doc: orchestration substrate options, hosting tiers, per-stage model selection. |
| **v0.8** | Split brownfield discovery into two dependencies — existing *code* (MCP) and existing *infrastructure* (Terraform → cloud API, CMDB parked). Named the concrete prerequisites to the first real `spec.md`. |
| **(2026-09-05)** | The Definition Agent phase specced (13 decisions). Docs restructured: `thinking/` → `notes/`, spec promoted into `architecture/`, `agents/`, `policies/`, `decisions/`. This file retired as the active index. |

## Non-goals (as currently scoped)

- Product discovery or prioritization — owned by the vertical's product function. *(The platform may draft product-level articulation; the prioritization/ROI decision stays with humans — see [`../decisions/log.md`](../decisions/log.md) D-17.)*
- Rebuilding provisioning, testing frameworks, data quality tooling, or CI/CD where those already exist — owned by the sibling enablers.
- Full autonomous deploy in v1 — deferred given assumed ticket-based fulfillment.

---

*Status: retired 2026-09-05. Not updated further — see the structured docs above. Kept for the version history and as the record of the conversation this design came out of.*
