# Business Brief — Enterprise Data Quality & Governance Platform

**Artifact:** Business brief (L1) · Artifact #1 of 4 in the chain · [← worked-example](README.md) · [chain defined in front-of-flow.md](../../architecture/front-of-flow.md#part-2--the-artifact-chain)

**Source note, read before anything else:** unlike the previous (now replaced) dance-studio brief, there is no direct sponsor statement behind this one. The subject matter is the `govern-iq` project — a sibling repo at `govern-iq/thnking/` containing an already-fairly-detailed technical architecture for an agentic data-quality system (orchestrator, profiler, rule-author, governance, rule-registrar, issue-register, and metadata-inference agents). This brief is *reverse-translated* from that L3-ish material back up to L1 business language — the harder, less-supported direction the platform's own notes flag as underdeveloped ([front-of-flow.md](../../architecture/front-of-flow.md): "the current pipeline only does downward translation… the pipeline barely does this today"). Treat it as a stand-in for a sponsor conversation that hasn't actually happened, not as a confirmed need.

Two projects are in play here and should not be conflated: **govern-iq** is the *product* this brief is about — the thing being specified. VibeIQ's front-of-flow chain (this artifact and the ones after it) is the *process* being used to spec it.

## Sponsor

Inferred, not confirmed: an enterprise data/platform leader — a Chief Data Officer, VP of Data Governance, or head of a data platform team — responsible for the quality and trustworthiness of data across a large, growing collection of systems. `[inferred]` Nobody has actually confirmed this persona; it's a plausible reading of who would commission something like this, not a known fact.

## What's being asked, in business terms

Restated without the engineering vocabulary the source material uses freely (dataframes, schemas, JSON, bronze/silver/gold):

## Problem

- The organization's data lives across many different systems and formats — databases, warehouses, spreadsheets, files people upload — and there's no consistent way to know whether any of it is actually good.
- Bad or wrong data usually isn't caught until it causes a downstream problem: a bad report, a bad decision, a broken process.
- The rules for "what counts as good data" are written by hand today, one at a time. That doesn't keep up as the number of data sources grows or the data itself changes.
- There's no living, trustworthy map of what the organization's data actually means, where it comes from, or how different pieces relate to each other. That knowledge lives in people's heads, not in a system — and it's lost as people leave or data grows.

## Desired outcome

Tooling that looks at data automatically, learns what "normal" looks like and flags where it's broken, proposes quality checks on its own instead of requiring someone to write them by hand, has those checks reviewed before they're trusted, and turns anything it finds into clear, assigned follow-up work — while building up the missing map of the organization's data as a side effect, without anyone having to build that map by hand.

## What we don't know yet

For the dance-studio brief, the gaps were mostly about which features mattered most. Here, the source material is already feature-rich — the gaps are almost entirely about the *business case*, because none of it was written down:

- Why this matters *now* — an incident, the cost of manual rule-writing, a compliance push, scaling pain? Not stated anywhere in the source.
- Who specifically benefits, and who actually has authority to sign off — a named sponsor, not just an inferred persona.
- What success is worth in business terms — cost saved, risk avoided, time saved. Nothing is quantified anywhere in the source material.
- Budget or timeline.
- Whether this is for one enterprise's internal use, or intended as a product sold to others — materially changes almost everything downstream.
- Anything explicitly out of scope.

## Related precedent

None named. Unlike the dance-studio case (where The Studio Director was cited directly as the incumbent), the source material doesn't reference any existing data-quality or data-observability tool as a comparison point or a system to displace. Comparable tooling clearly exists in the market — worth researching once there's a real product definition to compare against — but naming candidates now, without a sponsor pointing at one, would be inventing precedent rather than finding it.

---

*Business brief — L1, reverse-translated from existing technical architecture notes in `govern-iq/thnking/` (a sibling repo — not part of this one and not checked out here), not from a direct business-sponsor statement. Status: draft, standing in for a sponsor conversation that hasn't happened — confirm or correct before treating it as real intake.*
