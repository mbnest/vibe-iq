# Product Definition Phase — Integration Options

*Working notes, 2026-09-05. Possibilities, not commitments. The question: an enterprise adopting this platform often already has product tooling (Jira Product Discovery, Aha!, Productboard, Azure DevOps, Confluence) and an established product process. How does the product-definition phase fit alongside that? The aim is flexibility and extensibility — the flow documentation doubles as an integration guide, and our own working implementation is one adapter among possible others.*

Companion to [`front-of-flow.md`](../architecture/front-of-flow.md), [`agents/definition-agent.md`](../agents/definition-agent.md), and [`product-definition-phase-open-questions.md`](product-definition-phase-open-questions.md). Consistent with how the platform already treats external systems — the backlog as an "integration seam" ([architecture-napkin.md](architecture-napkin.md)), the Project Twin as "not the system of record" ([project-twin.md](project-twin.md)), n8n as "glue, never the core" ([platform-runtime.md](../architecture/runtime.md)).

---

## The core move: separate the contract from the implementation

**The flow documentation** defines, tool-agnostically:

- The levels model (L1–L4) and per-requirement level + provenance tags
- The artifact chain, derivation rules, and upward traceability
- What each sign-off attests to, freeze semantics, and change-request semantics (request vs. change_request, the solution-sign-off threshold)
- The completeness check's criteria

**The implementation** is a set of swappable adapters behind named ports. Our build is the default adapter for each; an enterprise tool can be an alternative adapter.

| Port | Our build (default adapter) | Enterprise alternative |
|---|---|---|
| `ArtifactStore` | Versioned requirement records + prose view | Productboard / Aha! / Jira Product Discovery objects |
| `SignoffProvider` | Our gate + form | Jira workflow transitions, Aha! approvals, ADO approvals |
| `BacklogSink` | Our feature/story tables | Jira epics/features, ADO work items, GitHub issues |
| `IdentityProvider` | Our user table | Okta / AD / enterprise SSO |
| `NotificationSurface` | Project Twin | Slack, Teams, the tool's native notifications |

**Never delegated:** the Definition Agent itself — the L1→L2 translation, level/provenance tagging, derivation and staleness logic. That is the value, and it stays ours regardless of where the artifact is stored or approved.

---

## Integration-depth ladder (least to most coupled)

- **L0 — Standalone.** We run the whole phase; the frozen definition is exported (doc / PDF / link) into their world by hand. Zero integration, works day one.
- **L1 — One-way publish.** We own the artifact; push read-only copies + links into their tool as the roadmap / PRD surface.
- **L2 — Two-way sync on key objects.** Features / epics and sign-off status sync bidirectionally; content authored in our form, visible and status-editable in theirs.
- **L3 — Their tool is the UI, we are headless.** Their forms and approval workflows drive our agent via webhooks / API. The product person never leaves Jira / Aha!.
- **L4 — Embedded.** Our agent runs as a plugin / app inside their tool (Jira Forge app, Aha! integration, ADO extension).

Likely path: build toward **L1–L2**, document **L3–L4** as future integration options.

---

## The flexibility keystone: sidecar metadata

Even when artifact *content* lives in their tool, keep a **sidecar graph in our store** — derivation links, level tags, provenance tags, version lineage, staleness flags — keyed to their object IDs. That is the minimum ownership that keeps the derivation chain working no matter where the prose lives. Their tool holds the human-facing artifact; we hold the connective tissue that makes it a *chain* rather than a pile of documents.

This is the same pattern the platform already uses for the Project Twin: a layer over a store, not a parallel store.

---

## Coexisting with an existing product *process*, not just tooling

An enterprise product team already does discovery, prioritization, and PRD authoring. Positioning options:

- **Augment.** The agent + provenance / completeness / traceability discipline layered onto their existing PRD authoring. They keep their process; we add rigor and derivation.
- **Replace the translation step only.** They keep discovery and prioritization; our agent owns L1→L2 and the sign-off loop, then hands to their backlog.
- **Parallel path.** Our agent handles AI-originated initiatives (from the Signal Agent); humans keep the manual path; both converge on the same artifact schema.

The levels model maps onto whatever they already call their artifacts — we are naming a discipline they partly practice, not imposing a new vocabulary.

---

## Integration seams (where we touch their tooling)

| Seam | Direction | Mechanism |
|---|---|---|
| Intake | their idea / request object → our parse agent | webhook or poll |
| Output | our product definition → their epic / feature hierarchy | API push |
| Sign-off | their approval workflow → our gate state | listen for their state transition |
| Feature breakout | our feature list ↔ their feature / epic objects | sync |
| Change requests | their change / comment events → our change classification | webhook |

---

## Risks of leaning on their tooling

- Their objects may not support per-requirement records + tags → the sidecar absorbs it, but the form UX degrades.
- Their approval workflow likely will not distinguish mirror-back sign-off from product-introduced-content sign-off (see open-questions Q12) → we may still need a thin gate of our own on top.
- Staleness propagation across their objects depends on webhook reliability.
- Two systems of record = drift risk — the exact thing the artifact chain exists to fight. Mitigation: the sidecar plus a strict "one system of record per field" rule.

---

*Status: new 2026-09-05. Possibilities only — no adapter beyond our own default is scheduled. Revisit when a real enterprise adoption with existing product tooling is on the table.*
