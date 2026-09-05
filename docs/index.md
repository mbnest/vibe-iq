---
title: VibeIQ
---

Every enterprise has a "right way" to ship software — how requirements get clarified, how
architecture gets reviewed, which infrastructure is pre-approved, what compliance rules apply.
That knowledge is real but uncodified: it lives in senior people's heads, and every team
relearns it, badly, on every project.

**This platform encodes those golden paths once — as a governed pipeline of AI agents with
real human sign-off gates — so every business unit inherits them automatically.** Software
gets delivered faster and more consistently, with traceability and compliance built in,
without the central engineering team becoming the bottleneck.

> Pre-planning design work. Nothing is running yet — this site is the case for building it.

---

## Why now

- **AI coding tools sped up the wrong part.** They make one developer faster inside the editor. They don't change how work *enters* engineering (unclear asks) or *leaves* it (unreviewed risk). The bottleneck moved to the edges.
- **Agents are good enough to trust inside a bounded step** — not to hand the whole SDLC to. The missing product is the governance *around* the agents: what runs when, what a human must approve, what an agent may never decide.
- **Enterprises won't adopt autonomous agents without a governance story.** This platform *is* that story — every action tiered by risk, every artifact versioned and traceable, every consequential decision owned by a named human.

## The "so what"

| Outcome | How |
|---|---|
| **Less rework** | Requirements are mirrored back and signed off before design or code — the "that's not what I asked for" failure is caught on day one, not in UAT. |
| **Faster delivery** | The pipeline runs the mechanical 80% at agent speed; humans spend time only on judgment and approval. |
| **Consistency across teams** | Golden paths encoded centrally — same architecture rigor, review depth, and compliance handling regardless of a team's own seniority. |
| **Audit by construction** | Every line of code traces upward to a signed business need. Regulatory constraints applied automatically, not remembered. |
| **The central team scales** | Pre-approved infrastructure becomes self-service within policy; only genuinely novel requests reach a human. |
| **Closed loop** | Post-release, the platform checks whether the change actually moved the business metric it was justified by. |

## The pipeline at a glance

```
FRONT OF FLOW  (per initiative)
  Signal → Scoping → [prioritize] → Definition Agent → Solution analysis
  → SOLUTION SIGN-OFF (human) → backlog

BUILD LOOP  (per story)
  Intake → REQUIREMENTS SIGN-OFF (human) → Design → Planning
  → Implementation → Test → Review → MERGE / RELEASE APPROVAL (human)
  → Enablement → Deployment → Post-release Validation
```

Three human gates, by design. Deterministic orchestration — the AI reasons *inside* a step and produces an artifact; it never decides the path.

## Suggested build-out order

1. **MVP — prove the thesis.** Intake → Requirements Sign-off → Design. Smallest slice that shows a governed agent pipeline producing real artifacts a human signs.
2. **The differentiator.** The Definition Agent (business ask → validated product definition) + the traceable artifact chain + Solution Sign-off. Where the value story is strongest and competitors are weakest.
3. **Close the build loop.** Planning → Implementation → Test → Review → Merge gate. Now: ask in one end, reviewed PR out the other. This is the demo.
4. **Ship it.** Enablement → Deployment → Validation. Highest external dependency; turns "PR ready" into "live and validated."
5. **Widen the funnel.** Signal + Scoping — only worth it once the pipeline behind them is trusted.

Full detail, per-feature value, and build sizing: **[Product Vision →](product-vision.md)**

---

## Dig in

| | |
|---|---|
| **[Overview](overview.md)** | The whole architecture in one read |
| **[Product Vision](product-vision.md)** | Executive "so what" — value per feature, build order |
| **[Process Flow](architecture/process-flow.md)** | The lifecycle and master diagram |
| **[Front of the Flow](architecture/front-of-flow.md)** | Requirement levels, the artifact chain, the fan-out |
| **[Agents & Gates](agents/)** | One spec per agent — purpose, inputs/outputs, autonomy, handoffs |
| **[Data Model](architecture/data-model.md)** | How artifacts are stored, versioned, linked, tagged |
| **[Runtime](architecture/runtime.md)** | Orchestration, hosting, model selection, UI |
| **[Policies](policies/)** | Autonomy tiering, requirement-change handling, governance vs. business rules |
| **[Decision Log](decisions/log.md)** | Every decision, one line each |
| **[Open Questions](decisions/open-questions.md)** | What's still undecided, and the top blocker |
| **[Worked Example](examples/govern-iq/)** | A real ask run through the artifact chain |
| **[Notes](notes/)** | The reasoning trail — the pre-planning conversations |

<p style="margin-top:2rem;color:#666;font-size:.9em">
Design work by Matt Nestman. Working positions, open to challenge — not a reviewed or committed design.
</p>
