# Product Definition — Enterprise Data Quality & Governance Platform

**Artifact:** Product definition (L2) · Artifact #2 of 4 in the chain · [← worked-example](README.md) · derived from [`01-business-brief.md`](01-business-brief.md)

Provenance tags follow the convention in [`agents/03-intake-agent.md`](../agents/03-intake-agent.md), with one substitution: since there's no live sponsor for this ask, `[stated]` here means *directly reflected in the `govern-iq/thnking/` source material*, not spoken by a person. `[default]` is a product-function judgment call filling a gap the source left open; `[inferred]` is read between the lines; `[unresolved]` needs a real sponsor and wasn't defaulted.

## Mirror-back

The need, from `01-business-brief.md`: automatically understand data quality across a large, heterogeneous set of data assets, propose and govern quality rules instead of hand-writing them, turn problems into actionable work, and build up institutional knowledge of the data (what it means, where it comes from, how it relates) as a byproduct. Below is that need turned into product requirements — grounded almost entirely in the existing `govern-iq/thnking/` source, since it's unusually detailed for a "business" ask. The gaps are concentrated in scope forks and success measurement, not features.

## Scope decisions

The source material describes a capable system but leaves several product-level forks unresolved. Defaulted here, correctable at sign-off:

- **Internal tool vs. external product.** `[unresolved]` — not product's call to default. Whether this is built for one enterprise's own data estate or as something sold to others changes multi-tenancy, security, and support requirements throughout. Flagged, not defaulted.
- **Human-in-the-loop for rule approval.** `[unresolved]` — the source names a Governance Agent that "validates, approves, rejects" candidate rules, but never says whether approval requires a human or can be fully agentic for some rule classes. This is structurally the same question the platform's own notes already work through for its *own* pipeline gates ([rules-and-policy.md](../rules-and-policy.md), decision 10: human gates stay human by design) — worth deciding by the same logic rather than reinventing it, but not defaulted here since it's consequential.
- **Target rule store / assertion framework.** `[unresolved]` — Rule Registrar Agent writes to "the enterprise rule store or assertion framework," unnamed. Needs a real target (e.g. Great Expectations, dbt tests, Soda, a custom store) before this is buildable.
- **Target issue-tracking system.** `[unresolved]` — Issue Register Agent produces "human-actionable issues" with owner/team assignment, but no ticketing system is named. Same integration-seam question the main platform notes raise for their own backlog ([vibe-engineering-platform-notes.md](../vibe-engineering-platform-notes.md), assumptions).
- **`dg_assessment_agent` / `ai_governance_agent`.** `[default]` Out of scope for v1 — the source README lists both as "optional extension," and neither has a spec file in the folder. Treated as future scope, not a v1 gap.
- **Scale target.** `[inferred]` "Entire schemas / entire databases" as a supported input implies enterprise-scale data volume, not a handful of spreadsheets — carried into NFRs below.

## Product requirements

Organized by the seven components already named in the source material — this maps close to one-to-one, since the source is already component-level, not because product invented this structure.

### 1. Asset discovery & normalization (Orchestrator)
- Discover data assets across supported types and normalize each into a canonical dataframe representation. `[stated]`
- Maintain an asset registry and execution/progress tracking across a run. `[stated]`
- Per-asset parallel execution with retry logic; agents themselves are stateless. `[stated]`

### 2. Profiling
- Automatic schema, type, and semantic-type inference per asset. `[stated]`
- Distribution, missingness, uniqueness, and outlier/anomaly detection. `[stated]`
- Domain and relationship hints; drift detection when historical data is available. `[stated]`

### 3. Rule authoring
- Generate candidate data-quality rules from profiling findings — range, regex, null, uniqueness, referential checks. `[stated]`
- Infer thresholds and attach a confidence score per rule. `[stated]`
- Suggest rule placement (e.g. which processing layer) and domain- or entity-level scope. `[stated]`

### 4. Governance
- Validate candidate rules for naming convention, duplication, conflicts, placement, and domain/semantic alignment before they take effect. `[stated]`
- Produce approved and rejected sets plus a change log — an audit trail of what was accepted or rejected and why. `[stated]`
- Target: catch conflicting or duplicate rules before they reach production, not after. `[default]`

### 5. Rule registration
- Write approved rules into the target rule store, organized by domain, entity, and semantic type. `[stated]`
- Emit an event when a rule is registered, for downstream monitoring systems to react to. `[stated]`

### 6. Issue management
- Convert findings and rule violations into actionable issues: severity, domain, entity, and owner/team assigned automatically. `[stated]`
- Generate a remediation suggestion alongside each issue, not just a bare notification. `[stated]`

### 7. Metadata, ontology & lineage
- Progressively infer and build an ontology, lineage graph, and semantic schema from findings and rules over time, rather than requiring this to be modeled by hand up front. `[stated]`
- Measure metadata deltas run-over-run — i.e. track how much of the unknown has been resolved, not just produce a static snapshot. `[stated]`

## Non-functional requirements

- Must operate across heterogeneous asset types at enterprise scale — individual files up through entire schemas/databases. `[stated]`
- Deterministic, retryable orchestration with per-asset parallelism and execution logging. `[stated]`
- Every governance decision (approve/reject) is auditable via a change log. `[stated]`
- Security/access: profiling and rule data will routinely touch sensitive or regulated columns simply by scanning entire schemas. `[inferred]` — no access-control or data-classification behavior is described anywhere in the source; flagged as a likely gap, not a confirmed requirement.

## Success metrics

`[unresolved]` — entirely absent from the source material, which is unusual for how detailed everything else is. Candidate metrics worth putting to a real sponsor, not yet confirmed:
- Reduction in hours spent hand-authoring data-quality rules
- Time-to-detect a data quality issue (vs. today's undetected-until-downstream pattern)
- % of enterprise data assets actively profiled and covered by at least one governed rule
- Rate of metadata/ontology completeness over time (ties to the "metadata delta measurement" behavior already specified)

## Constraints carried forward

- Rule approval must be auditable (change log). `[stated]`
- Likely touches sensitive/regulated data purely by scanning whole schemas — needs a real `constraints.md` entry once the specific enterprise/regulatory context is known. `[inferred]`
- Human-vs-agent authority over rule approval is unresolved and directly affects how much autonomy this system is allowed — carries forward as a constraint on the L3 solution, not just a nice-to-know. `[unresolved]`

## Open items for sponsor confirmation

1. Internal tool or external product — changes multi-tenancy, security, and support scope throughout
2. Whether rule governance requires a human approver, or can be agentic for defined-safe rule classes
3. Target rule store / assertion framework
4. Target issue-tracking system
5. Business case: why now, who benefits, what it's worth
6. Budget and timeline
7. Success metrics — none exist yet; the four candidates above need a real sponsor's confirmation or replacement
8. Data sensitivity / access-control requirements, since whole-schema scanning will touch regulated data by default

---

*Product definition — L2, drafted by agent + product function from `01-business-brief.md`, cross-referenced directly against `govern-iq/thnking/`. Per [front-of-flow.md](../front-of-flow.md), would normally be confirmed by the business sponsor (the validation loop) before feeding Solution Spec (L3, `03-solution-spec.md`) — here, that confirmation step is still owed since no sponsor conversation happened yet. Status: awaiting confirmation.*
