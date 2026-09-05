# Worked Example — Enterprise Data Quality & Governance Platform (govern-iq)

*Started 2026-09-04, retargeted 2026-09-04. A real (if illustrative) ask, run through the front-of-flow artifact chain one level at a time, to produce a concrete worked example the notes have been missing — and to serve as a stable reference for later A/B comparison (e.g. of alternate `spec.md` schemas or agent prompts) against a known-good chain.*

This directly resolves the "what is the first feature" prerequisite in [`../../decisions/open-questions.md`](../../decisions/open-questions.md): rather than dogfooding the Intake Agent's own implementation, this hand-simulates the front-of-flow chain against a real ask.

**Retargeted from an earlier dance-studio version.** The chain originally ran against a dance-studio scheduling/billing/marketing ask. That's been replaced entirely — see "Why govern-iq instead" below — rather than kept as a second example, per the decision to have one worked example at a time.

## Two projects, not one

- **govern-iq** — a sibling repo (`govern-iq/thnking/`) containing an already-detailed technical architecture for an agentic data-quality system (orchestrator, profiler, rule-author, governance, rule-registrar, issue-register, metadata-inference agents). This is the *product* the chain below is specifying.
- **vibe-iq** (this repo) — the SDLC pipeline/process being exercised by running govern-iq through it. The chain artifacts live here, not in govern-iq's own repo, because they're process output, not product source.

## Why govern-iq instead of the dance studio

The dance-studio brief started from a real sponsor's own words and worked downward (L1→L2), which is the direction the platform's pipeline already assumes. govern-iq is the opposite and harder case: existing technical material already sits at roughly L3, with no business brief behind it, so building the chain means translating *upward* first (L3→L1) before the normal downward pass can even start. [front-of-flow.md](../../architecture/front-of-flow.md) already flags upward translation as the direction "the pipeline barely does this today" — this is a chance to actually exercise it instead of leaving it theoretical.

## The chain

Per the artifact chain defined in [`front-of-flow.md`](../../architecture/front-of-flow.md) (Part 2):

| # | File | Level | Status |
|---|---|---|---|
| 1 | [`01-business-brief.md`](01-business-brief.md) | L1 | Draft — reverse-translated from `govern-iq/thnking/`, no real sponsor confirmation yet |
| 2 | [`02-product-definition.md`](02-product-definition.md) | L2 | Draft — awaiting confirmation (8 open items) |
| 3 | `03-solution-spec.md` | L3 | Not started — this is where `govern-iq/thnking/`'s existing content properly belongs; reconciling the two is pending |
| 4 | `04-spec.md` | L3→L4 | Not started |

Each artifact is drafted, then confirmed (or revised) by whoever the chain says holds that sign-off, before the next one is drafted from it — no artifact skips ahead of the one it derives from.

## The ask, in one line

An (inferred, unconfirmed) enterprise data leader wants a system that automatically profiles data quality across many heterogeneous sources, proposes and governs quality rules instead of hand-writing them, turns problems into actionable work, and builds institutional data knowledge (ontology/lineage) as a byproduct.

---

*Doc status: retargeted 2026-09-04 — artifacts #1 and #2 rewritten against govern-iq, dance-studio versions removed. This is a working exercise, not a commitment to build or ship govern-iq as specified here — treat it as a test fixture for the pipeline design.*
