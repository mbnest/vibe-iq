# Definition Agent

**Pipeline position:** Front of the flow, between [Scoping](02-scoping-agent.md) (step 2) and [Intake](03-intake-agent.md) (step 3). Part of the v0.5 front-of-flow evolution ([`front-of-flow.md`](../front-of-flow.md)), not yet numbered in the master diagram. · [← back to master](../sdlc-business-process-flow.md)

*New 2026-09-05. Captures a series of design conversations between Matt and Claude. Draft — a working position to challenge, like every other agent doc.*

## Purpose

Translate an already-prioritized business ask from L1 (business / sponsor language) to L2 (product language) — producing the **product definition**, artifact #2 in the chain. This is the pipeline's answer to "which agent is aligned to product": the L1→L2 hop is the product-management discovery-and-definition function, and it is the highest-risk translation in the pipeline because it *creates* information the business never stated (precision, targets, NFRs, scope boundaries).

Naming follows the platform convention — agents are named for the **transform they own** (brief → definition), not the persona they serve. Persona / role belongs in the sign-off column, not the agent name.

## Two motions, one output

The agent does two distinct jobs that share an artifact:

- **Mirror-back (fidelity).** Restate the L1 need at L2 so the stakeholder can confirm "yes, that is what I meant." Upward-facing, must not add or distort.
- **Augmentation (product judgment).** Add the precision the business did not state — requirements, NFRs, targets, success metrics, personas, feature breakout, scope boundaries, risks. Product-owned.

These carry different accountability: the stakeholder signs off **only** the mirror-back; product separately attests to the augmentation (open-questions Q12).

## Inputs

- Business brief / intake request (L1), from [Scoping](02-scoping-agent.md), checked against the minimum intake scaffold ([Intake Agent](03-intake-agent.md))
- Current roadmap, OKRs, strategy context (where reachable)
- Existing product / capability inventory (the [Project Twin](../project-twin.md), where it exists)
- Historical cost / actuals, analytics baselines, CRM / segment data (long-term — see the autonomy demarcation below)
- Responses from whichever stakeholder or authority a gap was routed to

## Outputs

- **`product-definition.md`** (L2) — structured requirement records (Q1) plus a generated prose view. Each requirement carries: `id`, text, origin (`stakeholder` / `product-introduced`), level tag, provenance tag, trace link to an L1 need.
- **Feature breakout** — the initiative decomposed into candidate features, curated by product, part of the frozen artifact (Q4).
- **Open-items list** — everything owed to the sponsor before or at sign-off.
- **Sidecar metadata** — derivation links, version lineage, staleness flags (portable across storage backends — see [`product-definition-integration-options.md`](../product-definition-integration-options.md)).

## The phase loop

```
raw request
  → [scaffold pre-check]  fill gaps from context; bounce only irreducible gaps to stakeholder (Q9)
  → agent drafts product-definition  (publish safe sections, flag risky ones — Q6)
  → product revises via form + chatbot sidekick  (sidekick writes to fields directly, user confirms on submit — Q8)
  → [completeness check]  agent runs fixed checklist; commit blocked until pass or logged product override (Q10)
  → product commits  (attestation + state transition draft → in-review; not a governance gate)
  → stakeholder(s) review  (all affected parties — Q11)
  → stakeholder sign-off  (attests to mirror-back fidelity only — Q12)
      ├─ edit requested → back to product; product may decline with rationale (Q7); new draft version
      └─ signed → state transition in-review → official (frozen)
```

The whole loop before freeze runs on draft versions — **no `change_request` records**. It is draft churn, not change management.

## The artifact model

- **Structured records underneath prose** (Q1). A form needs fields to bind to; mechanical staleness propagation needs discrete requirements.
- **Both request and product-definition are versioned.** Every product-definition version points to exactly one *cause* — an `initial` request version or a `change_request` version. `request` and `change_request` are the same table with a type discriminator.
- **Freeze semantics.** Can freeze carrying `[unresolved]` on *minor* scope forks (become inherited risk); a fork tagged **load-bearing** blocks the freeze until an authoritative source resolves it (Q2). Success-metric *definitions* must be present and *target values* must be filled by agent or person — no `[unresolved]` target at freeze (Q3).
- **Level tags inform, they do not bind** (Q5). Every requirement is tagged with its precision level, but the responsible layer owner (product, at L2) keeps decision autonomy — the tag is a signal, never an automated gate.
- **Provenance tags** reuse the Intake vocabulary (`[stated]` / `[default]` / `[inferred]` / `[unresolved]`) with a documented rebinding for the no-live-sponsor case (Q13).

## Autonomy demarcation — what the agent does now, later, and never

| Function | Now (agent) | Later, given data access | Always human |
|---|---|---|---|
| Mirror-back | Draft the L2 restatement | — | Sponsor confirms fidelity |
| Scope forks | Enumerate and frame every fork | — | Decide the load-bearing ones |
| Assumptions / risks / deps | Draft comprehensively | + predict from comparable past initiatives | Add domain / political risks only they see |
| NFRs (what-level) | Draft; flag regulated-data touches | Ground against infra + compliance registries | Confirm the risky ones |
| Success metrics | Propose definitions; targets `[unresolved]` transiently | Propose target values from real baselines | Ratify the ambition level |
| Personas / segments | Draft candidate personas | Quantify actual affected segments | Confirm intended target |
| Feature breakout | Propose a decomposition | Propose the v1 cut + sequencing | Approve the cut |
| Strategic alignment | Flag visible conflicts only | Full alignment analysis vs. strategy + portfolio | Own the "this is a priority" call |
| Prioritization rationale | Rough effort / cost sketch | Full ROI case with real numbers | Own the portfolio decision |
| Constraints (budget / time / reg) | Record what humans supply; flag obvious reg touches | Propose an envelope from historical actuals | Set the actual authority |

**The bridge from "now" to "later"** is three separate efforts:

1. **Integrate systems of record** — strategy corpus, finance / PMO actuals, analytics / telemetry, CRM. Mostly retrieval plumbing and access control.
2. **Build the [Project Twin](../project-twin.md)** — the current-state model of products and capabilities that exist today.
3. **Accumulate the platform's own history** — every initiative it runs becomes a comparable. Early initiatives run human-heavy and pay into the corpus that makes later ones autonomous. No integration needed, just time and a schema.

**Permanent constraint:** decision rights never transfer with data. Full data access makes the agent a better *advisor* for the portfolio / ambition / budget calls — never the decider. Consistent with [rules-and-policy.md](../rules-and-policy.md) decision 10 (human gates stay human by design).

## Autonomy Tiering

| Quadrant | What it looks like here | Behavior |
|---|---|---|
| Known + Safe | Standard mirror-back, common requirement / NFR patterns, scope-fork enumeration, risk enumeration. | Full autonomy — draft and publish those sections directly (Q6). |
| Known + Risky | Requirements touch PII / regulated data, or the augmentation makes a technical decision that belongs to architecture. | Autonomous draft; flag the section for product before commit. |
| Unknown + Safe | A genuinely new product area, low stakes. | Autonomous draft; reviewed after, not gated before. |
| Unknown + Risky | Load-bearing scope fork only a sponsor can settle; strategic-alignment judgment with no strategy context; a target value with no baseline and no sponsor. | Manual — a human resolves it. The artifact carries `[unresolved]` (minor) or is blocked from freeze (load-bearing / missing target). |

## Escalation Triggers

- A scope fork classified load-bearing with no authoritative source identified
- A success-metric target value that neither the agent nor an available person can supply
- Stakeholders give conflicting answers to the same clarifying question
- The augmentation would commit a technical / architectural decision product does not own
- An affected party withholds sign-off past a defined window (Q11)
- The raw request fails the minimum intake scaffold and the gaps are not fillable from context

## Handoffs

- **Receives from:** [Scoping Agent](02-scoping-agent.md) (step 2), once the product function has made the prioritization decision.
- **Delivers to:** Solution analysis (front-of-flow) with the frozen, signed L2 product definition; then per-story [Intake](03-intake-agent.md) (step 3) inherits the relevant slice as constraint boundary.
- **Change requests** after product-definition sign-off re-enter here (cheap path) until solution sign-off passes, after which they route through [requirement-change-handling](../requirement-change-handling.md) (expensive path).

## Open sub-items (from [`product-definition-phase-open-questions.md`](../product-definition-phase-open-questions.md))

- Is the bounce of irreducible scaffold gaps agent-autonomous, or human-confirmed first? (Q9)
- The contents of the completeness checklist. (Q10)
- Where the agent sources baselines to fill target values. (Q3 / the bridge)
- The concrete "informs but does not bind" mechanism for level tags in a programmatic flow. (Q5)

---

*Doc status: new 2026-09-05, not reviewed by anyone else. Consolidates the product-definition-phase discussions; the 13 phase decisions live in the open-questions doc, integration possibilities in the integration-options doc. Next: reflect this agent in `front-of-flow.md` and the master diagram's step reference.*
