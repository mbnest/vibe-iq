# Architecture Decision Records

*An append-only log of consequential engineering decisions. Each ADR captures one decision: the context that forced it, what was decided, and the consequences accepted. ADRs are immutable once accepted — a later decision that changes course is a new ADR that supersedes the old one, which stays in place with its status updated.*

## Relationship to the engineering-context pack

ADRs are the decision log. The files in [`../engineering-context/`](../engineering-context/) are the current-state view derived from it. An ADR explains *why* `allowed-stack.md` says what it says; the pack file is what agents read for the current rule.

## When to write one

Write an ADR when a decision is consequential and contestable — it constrains future work, or someone later would reasonably ask "why is it built this way?" This includes deliberate MVP shortcuts: record the expedient choice, and put a **Revisit when:** trigger in the consequences.

Do not write one for trivial or easily reversed choices, or for anything a linter or formatter config already settles.

## Format

Markdown, one file per decision, named `NNNN-short-title.md` with a zero-padded sequence number. Use [`template.md`](template.md).

Sections: **Status** - **Context** - **Decision** - **Consequences**

## Status lifecycle

`Proposed` -> `Accepted` -> (later) `Superseded by ADR-NNNN` or `Deprecated`

A superseded ADR is never deleted or edited beyond its status line and a pointer to its replacement.

## Index

| ADR | Title | Status |
|---|---|---|
| [0001](0001-record-architecture-decisions.md) | Record architecture decisions | Accepted |
| [0002](0002-target-application-stack.md) | Target application stack for the MVP | Accepted |
| [0003](0003-allow-sqlite-for-local-and-low-volume-workloads.md) | Allow SQLite for local and low-volume workloads | Accepted |

*Status: initiated 2026-09-03.*
