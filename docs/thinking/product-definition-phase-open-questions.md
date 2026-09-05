# Product Definition Phase — Open Questions

*Working notes, 2026-09-05. Scoped strictly to the product-definition phase: the loop from a raw stakeholder request to a frozen, signed-off product definition. Upstream (submission mechanism) and downstream (solution sign-off, change handling) questions are out of scope here. Each question has options; answers are recorded inline as they are decided.*

Companion to [`front-of-flow.md`](front-of-flow.md) and the (proposed) Definition Agent. The phase produces artifact #2 (L2 product definition) from artifact #1 (L1 business brief / intake request).

---

## Decisions summary (answered 2026-09-05)

| # | Question | Decision |
|---|---|---|
| Q1 | Artifact structure | **Fully structured records** — each requirement a record; prose is a generated view |
| Q2 | Freeze with open forks | **Conditional** — minor forks carry as `[unresolved]` risk; load-bearing forks block the freeze |
| Q3 | Success-metrics bar | **Definitions required; targets must be filled** by agent or person before the artifact moves forward — no `[unresolved]` target at freeze |
| Q4 | Feature breakout | **Agent drafts, product curates, in the frozen artifact** |
| Q5 | Level tags | **Tag every requirement** — but tags *inform* the layer owner, they don't *bind*; process/docs must reflect owner autonomy |
| Q6 | Agent autonomy | **Publish safe, flag risky** — agent publishes mirror-back, scope-forks, risks, NFR draft, candidate personas/metrics/features; rest draft-and-flag |
| Q7 | In-review edit authority | **Product can decline** a stakeholder edit with rationale — product owns the artifact |
| Q8 | Chatbot sidekick | **Writes to fields directly** on the user's behalf; user confirms on submit |
| Q9 | Thin-request routing | **Fill what it can, bounce the rest** to the stakeholder |
| Q10 | Completeness gate | **Agent checks vs. fixed checklist** — commit blocked until pass or logged product override |
| Q11 | Multi-stakeholder sign-off | **All affected parties must sign**; tool must always show who has not yet approved |
| Q12 | Sign-off scope | **Fidelity of mirror-back only** — product-introduced content separately attested by product |
| Q13 | Provenance vocabulary | **Reuse Intake tags with a documented rebinding** for no-live-sponsor cases |

### Still open

- **Q9 sub-question** — is the bounce of irreducible gaps agent-autonomous, or does a human confirm "too thin" first?
- **Q10** — the contents of the completeness checklist itself.
- **Q3** — where the agent gets baselines to fill target values (ties to the long-term data-access bridge: analytics/telemetry, historical actuals).
- **Q5** — the concrete mechanism for "informs but doesn't bind" in a programmatic flow (surface the tag to the owner; never auto-gate on it).

---

## A. What the artifact must contain

### Q1 — Artifact structure depth

How structured is the product definition underneath its prose?

- **A.** Fully structured — each requirement is a record (`id`, text, origin, level tag, provenance tag, trace link). Prose is a generated view.
- **B.** Hybrid — prose is primary, but every requirement is an enumerable, ID'd line carrying inline tags.
- **C.** Prose only — tags inline, no per-requirement IDs.

**Answer:** **A** — fully structured records. Each requirement is a record; prose is a generated view.

---

### Q2 — Freezing with `[unresolved]` scope forks

Can the definition freeze while load-bearing scope forks are still open (e.g. "internal tool vs. external product")?

- **A.** Allowed — freezes carrying `[unresolved]`; each becomes a named risk/assumption inherited by solutioning.
- **B.** Blocked — cannot freeze until every fork is resolved by an authoritative source.
- **C.** Conditional — minor forks may carry; forks tagged "load-bearing" block the freeze.

**Answer:** **C** — conditional on severity. Minor forks carry as `[unresolved]` risk; forks tagged load-bearing block the freeze until resolved by an authoritative source.

---

### Q3 — Success metrics exit bar

What level of success-metric definition is required to freeze?

- **A.** Metric *definitions* required; target *values* may stay `[unresolved]`.
- **B.** Both definitions and target values required.
- **C.** May freeze with metrics entirely open, flagged as the top risk.

**Answer:** **A, hardened.** Metric *definitions* must be proposed. Target *values* must then be filled — by the agent (where it has baselines/data) or by a person. If neither can supply a target value, the artifact does **not** move forward until it is defined. So `[unresolved]` is permitted on a target only transiently, never at freeze.

---

### Q4 — Feature breakout: who and when

Where does the initiative get decomposed into features?

- **A.** Definition Agent drafts the feature list, product curates, it is part of the frozen artifact.
- **B.** Product carves features by hand, after freeze, as a separate step.
- **C.** Agent drafts candidate features inside the artifact; the authoritative cut happens at solutioning.

**Answer:** **A** — Definition Agent drafts the feature list, product curates, it is part of the frozen artifact.

---

### Q5 — Level tagging of output requirements

Does each requirement in the definition carry a precision-level tag?

- **A.** Every requirement tagged with its level (L1/L2/L3); mixed levels expected.
- **B.** No per-requirement level tags here — the whole artifact is "L2".
- **C.** Only flag the exceptions — requirements that arrived already at L3.

**Answer:** **A, with a principle attached.** Tag every requirement with its level. **But tags inform, they don't bind:** the responsible layer owner (product at L2) keeps autonomy to make decisions — those decisions are *informed by* the tags, not *beholden to* them. Documentation and process flow (manual or programmatic) must reflect this: a tag is a signal to the owner, never an automated gate that overrides their judgment.

---

## B. How the loop runs

### Q6 — Autonomy tiering for the L1→L2 hop

How much does the agent publish on its own vs. flag for product?

- **A.** Agent draft-and-publishes the mirror-back, scope-fork enumeration, risks, NFR draft, candidate personas/metrics/features; everything else draft-and-flag.
- **B.** Agent draft-and-flags everything — product must touch every section before commit.
- **C.** Per-requirement — known/safe requirements auto, novel/risky flagged (task-node granularity, like the rest of the platform).

**Answer:** **A** — agent publishes mirror-back, scope-fork enumeration, risks, NFR draft, candidate personas/metrics/features; everything else draft-and-flag.

---

### Q7 — Authority inside the in-review loop

When the stakeholder requests an edit during review, what governs?

- **A.** Product can decline with rationale — product owns the artifact.
- **B.** Product must apply edits to the *need*; may push back only on product-introduced content.
- **C.** Disagreement escalates to a named arbiter (scoping/product lead).

**Answer:** **A** — product can decline a stakeholder edit with rationale. Product owns the artifact. (Note: this is the *initial* definition loop. Post-freeze changes still follow the change-request path.)

---

### Q8 — Chatbot sidekick scope

What can the chat sidekick do to the form?

- **A.** Write directly to fields on the user's behalf (user confirms on submit).
- **B.** Draft text only — user accepts it into a field manually.
- **C.** Write to free-text fields directly; structured fields draft-only.

**Answer:** **A** — sidekick writes directly to fields on the user's behalf; user confirms on submit.

---

### Q9 — Scaffold pre-check failure routing

A raw request that fails the minimum intake scaffold goes where?

- **A.** Straight back to the stakeholder who submitted.
- **B.** Back to Scoping (step 2) for more work before re-entering.
- **C.** Agent fills what it can from context first; bounces only the irreducible gaps.

Sub-question: is the bounce agent-autonomous, or does a human confirm "too thin" first? _open_

**Answer:** **C** — agent fills what it can from available context first, then bounces only the irreducible gaps to the stakeholder.

---

### Q10 — Completeness check before product commit

Is there an automated completeness gate before product can commit?

- **A.** Agent runs a completeness pass against a fixed checklist; commit blocked until it passes or product overrides.
- **B.** No automated check — product's commit *is* the completeness judgment.
- **C.** Checklist exists as guidance, not enforced.

**Answer:** **A** — agent runs a completeness pass against a fixed checklist; commit blocked until it passes or product explicitly overrides with a logged reason.

---

## C. How it exits

### Q11 — Multi-stakeholder sign-off

Who signs when more than one party is affected?

- **A.** One named sponsor signs; other affected parties get visibility only.
- **B.** All affected parties must sign; conflicts escalate.
- **C.** Sponsor signs; affected parties may raise a blocking objection within a window.

**Answer:** **B** — all affected parties must sign; conflicts escalate before freeze. The tracking tool must clearly show which party or parties have **not** yet approved at any moment.

---

### Q12 — Sign-off record granularity

What does the stakeholder's sign-off actually attest to?

- **A.** Fidelity of the mirror-back only; product-introduced content is product-owned and separately attested.
- **B.** A single sign-off covers the whole artifact.
- **C.** Per-section sign-off — mirror-back section vs. augmentation section.

**Answer:** **A** — stakeholder attests to fidelity of the mirror-back only. Product-introduced content (NFRs, targets, features) is product-owned and separately attested by product.

---

### Q13 — Provenance vocabulary at L2

The worked example had to redefine `[stated]` for the "no live sponsor" case. What is the standing rule?

- **A.** Reuse Intake's tags (`[stated]`/`[default]`/`[inferred]`/`[unresolved]`) with a documented rebinding for no-live-sponsor cases.
- **B.** Define a distinct L2 vocabulary.
- **C.** Reuse as-is, no rebinding — accept the imprecision.

**Answer:** **A** — reuse Intake's tags (`[stated]`/`[default]`/`[inferred]`/`[unresolved]`) with a documented rebinding of what each means when there is no live sponsor.

---

*Status: new 2026-09-05. All 13 questions answered (see Decisions summary at top); 4 sub-items still open. Next step: fold these into a Definition Agent doc (`agents/` template) and reflect the phase in `front-of-flow.md` / the master diagram.*
