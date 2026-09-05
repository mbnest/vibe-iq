# Product Vision — What This Is, Why It Matters, What to Build First

*The executive read: the product in one page, the value of each part, and the order to build it. Detail lives in [`overview.md`](overview.md) and the linked specs; this doc is the "so what."*

---

## The one-paragraph pitch

Every enterprise has a "right way" to ship software — how requirements get clarified, how architecture gets reviewed, which infrastructure is pre-approved, what compliance rules apply. That knowledge is real but uncodified: it lives in senior people's heads, and every team relearns it, badly, on every project. **This platform encodes those golden paths once, as a governed pipeline of AI agents with real human sign-off gates, so every business unit inherits them automatically.** The result is software delivered faster and more consistently, with traceability and compliance built in — without the central engineering team becoming the bottleneck.

## Why it matters now

- **AI coding tools sped up the wrong part.** They make an individual developer faster inside the editor. They don't change how work *enters* engineering (unclear asks) or how it *leaves* (unreviewed risk). The bottleneck moved to the edges.
- **Agents are now good enough to trust inside a bounded step** — but not to hand the whole SDLC to. The missing product is the governance and orchestration *around* the agents: what runs when, what a human must approve, what an agent may never decide.
- **Enterprises won't adopt autonomous agents without a governance story.** This platform *is* that story: every action is tiered by risk, every artifact is versioned and traceable, every consequential decision has a named human owner.

## The "so what," by outcome

| Business outcome | How the platform delivers it |
|---|---|
| **Less rework** | Requirements are mirrored back and signed off at the cheapest point to catch a misunderstanding — before design, before code. The "that's not what I asked for" failure is caught on day one, not in UAT. |
| **Faster time-to-delivery** | The pipeline runs the mechanical 80% (spec drafting, decomposition, boilerplate, test authoring, standard infra) at agent speed; humans spend their time only on judgment and approval. |
| **Consistency across teams** | Golden paths are encoded centrally. Every vertical gets the same architecture rigor, the same review depth, the same compliance handling — regardless of how senior their own engineers are. |
| **Audit & compliance by construction** | Every line of code traces upward to a signed business need. Regulatory constraints are applied automatically at intake, not remembered. Every gate decision is logged. |
| **The central team scales** | The enablement team stops being a ticket queue for the common case. Pre-approved infrastructure is self-service within policy; only genuinely novel requests reach a human. |
| **Closed feedback loop** | Post-release, the platform checks whether the change actually moved the business metric it was justified by — a check almost nobody does today. |

---

## What makes up the product

Each agent is an independent feature with its own value. Grouped by the two phases of the pipeline. "Build size" is rough: **S** = days, **M** = 1–2 weeks, **L** = weeks, **XL** = weeks + external integration.

### Front of the flow — turning a business ask into a buildable backlog

| Agent / Gate | What it does | Business value — the "so what" | Build size | Key dependency |
|---|---|---|---|---|
| [Signal Agent](agents/signal-agent.md) | Scans support tickets, telemetry, and market sources for opportunities worth building | Nothing valuable slips through the cracks; opportunity cost of missed signals is invisible but large | M | Data-source access (support, analytics) |
| [Scoping Agent](agents/scoping-agent.md) | Turns an opportunity into a sized, prioritized ask with ROI and roadmap-fit analysis | Leadership triages with consistent decision inputs instead of gut feel; faster, defensible prioritization | M | Roadmap / OKR data |
| [Definition Agent](agents/definition-agent.md) | Translates a business ask (L1) into a validated product definition (L2): requirements, NFRs, success metrics, feature breakout — mirrored back to the sponsor | **The differentiator.** The translation that normally costs a PM weeks of meetings, done in a day, with every requirement traceable to a stated need and tagged for how solid it is | L | The artifact-chain data model |
| [Solution Sign-off](agents/solution-signoff-gate.md) | Human gate: an architect approves the high-level solution before stories are written | Catches the expensive mistake — bad architecture — before 50 tickets are built against it | S | Definition Agent output |

### Build loop — turning a backlog item into a validated production change

| Agent / Gate | What it does | Business value — the "so what" | Build size | Key dependency |
|---|---|---|---|---|
| [Intake Agent](agents/intake-agent.md) | Resolves engineering ambiguity, enumerates edge cases, applies compliance constraints, produces `spec.md` with acceptance criteria | A spec a senior engineer would have written — every time. Compliance rules applied automatically, not left to memory | L | `spec.md` schema (open blocker); constraint registry |
| [Requirements Sign-off](agents/requirements-signoff-gate.md) | Human gate: stakeholder confirms the spec matches intent | Cheapest possible catch point for a misunderstanding | S | Intake output |
| [Design Agent](agents/design-agent.md) | Turns the spec into a technical design within the signed solution; documents tradeoffs as ADRs | Consistent architecture decisions; a written rationale for every choice, produced as a byproduct rather than a chore | L | Engineering-context pack; brownfield context |
| [Planning Agent](agents/planning-agent.md) | Decomposes the design into a sequenced, parallelizable task graph | Mechanical breakdown with no project-management overhead; identifies what can run concurrently | M | Design output |
| [Implementation Agent(s)](agents/implementation-agent.md) | Builds the change per the task graph, running parallel branches where possible | The actual code, at agent speed — but bounded by an approved design, not free-forming | L | Task graph; brownfield context |
| [Test Agent](agents/test-agent.md) | Derives tests from the spec, independent of the code | No self-grading. Coverage is tied to what was *required*, not to what happened to be written | M | Spec schema; sibling testing enabler |
| [Review Agent](agents/review-agent.md) | Independent review of spec + diff, from different context than the implementer | Catches correlated blind spots; the same review rigor regardless of who is available that day | M | Implementation + test output |
| [Merge & Release Approval](agents/merge-release-approval-gate.md) | Human gate: an accountable person approves before production | A named owner on every production change | S | Review output |
| [Enablement Agent](agents/enablement-agent.md) | Provisions infrastructure from a pre-approved catalog; files a fully-contexted ticket for anything novel | Self-service infra *within policy*. The central team stops being a ticket queue for the 80% common case | XL | Vetted module catalog; policy-as-code; sibling enablers |
| [Deployment Agent](agents/deployment-agent.md) | Ships the change; makes the rollback judgment call | Consistent deploy discipline; faster recovery when something goes wrong | M | CI/CD enabler |
| [Validation Agent](agents/validation-agent.md) | Confirms the change behaves in production and moved its target metric | Closes the loop: did we actually deliver the business outcome we justified this with? | M | Metrics/telemetry access |

### Cross-cutting assets — sellable in their own right

| Asset | What it is | Business value |
|---|---|---|
| [Artifact chain + traceability](architecture/data-model.md) | Every artifact derived from the one above, versioned, upward-linked | Change a business need and the system tells you exactly what downstream is now stale. Audit trail from code to rationale. |
| [Autonomy tiering](policies/autonomy-tiering.md) | A risk model (known-path? × safe?) applied per task, deciding what an agent may do alone | The governance framework that makes "let agents do it" acceptable to an enterprise risk officer |
| [Engineering-context pack](engineering-context/) | The org's standing rules — stack, principles, constraints, standards — read by agents at each stage | Golden paths encoded once, centrally, versioned |
| [Requirement-change handling](policies/requirement-change-handling.md) | Classifies a mid-flight change by blast radius and re-enters the pipeline at the right depth | Changes don't silently rot the plan or force a full restart |

---

## Suggested build-out order

The ordering principle: **prove the core thesis on the smallest slice, then extend along the axis of highest differentiation before the axis of highest external dependency.**

### Phase 0 — MVP: prove the thesis (now)

**Intake → Requirements Sign-off → Design.** Greenfield, one technical reviewer, runs on the command line, git for storage.

*Why first:* the smallest slice that demonstrates the whole idea — a governed agent pipeline producing real, versioned artifacts that a human signs. No orchestration, no UI, no external integrations. If this isn't compelling, nothing downstream matters.

### Phase 1 — The front-of-flow differentiator

**Definition Agent + the artifact chain + Solution Sign-off.**

*Why next:* this is the part competitors don't do well and where the executive "so what" is strongest — killing rework and delivering traceability. It's also the first time a non-technical person (a business sponsor) has to use the system, which forces the first real UI. High value, and it de-risks the hardest design question (the L1→L2 translation).

### Phase 2 — Close the build loop

**Planning → Implementation → Test → Review → Merge gate.**

*Why next:* now the platform runs end-to-end from "ask" to "reviewed PR ready to merge." This is the demo that sells the whole vision — a business request going in one end and mergeable, tested, reviewed code coming out the other.

### Phase 3 — Ship it

**Enablement → Deployment → Validation.**

*Why here:* highest external dependency (sibling enabler teams, real cloud infrastructure, policy-as-code). Turns "PR ready" into "live and validated in production." Deferred until the pipeline producing those PRs is trusted.

### Phase 4 — Widen the funnel

**Signal → Scoping.**

*Why last of the agents:* these feed the top of the pipeline. They're only worth building once the pipeline behind them is proven — an opportunity surfaced by the Signal Agent is worthless if the rest can't act on it. Also needs the most data-source integration work.

### Phase 5 — The operations tail

**Incident Response · Maintenance & Iteration · Deprecation / Sunset.** Not yet designed — the same scoping exercise applied to the build cycle hasn't been done here.

---

## The honest caveats

- This is a **pre-planning design**, not a running system. The value proposition is argued, not yet proven.
- The **named top blocker** is the `spec.md` schema — how structured the core artifact needs to be underneath its prose. Several agents depend on the answer.
- Every **external dependency** (the sibling enabler teams, brownfield code context) is an assumption tracked individually, not a confirmed fact. Where one turns out not to exist, that capability becomes build scope.
- **Autonomy tiers per agent** are starting proposals, not validated against real historical change data.

---

*See [`decisions/log.md`](decisions/log.md) for every decision behind this, [`overview.md`](overview.md) for the architecture, and [`notes/`](notes/) for the reasoning trail.*
