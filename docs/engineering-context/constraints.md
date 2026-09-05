# Constraints

*Regulatory and policy obligations that bind what may be built. Read — and **queried** — by the Intake stage: the Intake agent selects every constraint whose **applies-when** rule matches an ask and cites it by ID in `spec.md` as an acceptance criterion.*

> **This file needs real input.** Matt is providing the actual regulatory and policy context — regulations in scope, data residency requirements, industry rules, internal security policy. The two entries below are illustrative examples showing the required shape. Replace them.

## Convention

Each constraint is one block:

- **ID** — stable, category-prefixed (`PII-1`, `SEC-2`, `RET-1`). Never reused or renumbered.
- **Statement** — the obligation, in one or two sentences.
- **Source** — the regulation, standard, contract, or internal policy it comes from.
- **Applies when** — a plain-language rule the Intake agent evaluates against an ask. Precise enough to select on.
- **Verification** — how adherence is confirmed downstream.

Specs cite constraints inline, for example:
`AC-7: Card data is tokenized before storage. [constraint: PCI-1] [stated]`

Likely categories: `PII` (data protection), `SEC` (security), `RET` (retention), `AUDIT` (logging and audit), `ACCESS` (authorization), `RESIDENCY` (data location).

---

## PII-1 — Personal data encrypted at rest *(EXAMPLE — replace)*

- **Statement.** Any datastore holding personal data of an identifiable individual must have encryption at rest enabled, using a customer-managed key.
- **Source.** *(placeholder — e.g. GDPR Art. 32, or internal data-protection policy)*
- **Applies when.** The ask involves storing or persisting any field that identifies a person (name, email, address, phone, government ID, precise location) or can be trivially linked to one.
- **Verification.** Terraform review confirms `storage_encrypted = true` with a customer-managed key; design review confirms the data classification.

## SEC-1 — No secrets in source or plain config *(EXAMPLE — replace)*

- **Statement.** Credentials, API keys, tokens, and connection strings are retrieved at runtime from AWS Secrets Manager. They never appear in source, container images, or unencrypted environment files.
- **Source.** *(placeholder — internal security policy)*
- **Applies when.** Always. Every ask.
- **Verification.** Secret scanning in CI; code review.

---

*Status: skeleton plus examples, 2026-09-03. Awaiting the real constraint set from Matt.*
