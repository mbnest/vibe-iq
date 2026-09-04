# ADR-0001: Record architecture decisions

## Status

Accepted - 2026-09-03

## Context

This project is a learning vehicle and a moving design. Decisions are being made
continuously - some firm, some deliberately expedient for the MVP. Without a
record, the reasoning behind a choice is lost, and a future reader (human or
agent) cannot tell a considered decision from an accident, or a permanent choice
from a shortcut.

## Decision

We will keep Architecture Decision Records, one Markdown file per decision, in
`docs/adr/`, using the format in `template.md` (Status / Context / Decision /
Consequences). ADRs are append-only: superseding a decision means a new ADR, not
an edit to the old one.

The engineering-context pack in `docs/engineering-context/` is maintained as the
current-state view derived from these decisions.

## Consequences

- Every consequential or contestable choice has a durable rationale, including
  deliberate MVP shortcuts.
- Small overhead per decision, mitigated by keeping ADRs short and skipping them
  for trivial or linter-settled choices.
- In any working session on this project, the question "does this need an ADR?"
  is raised when a decision is made.

**Revisit when:** the ADR practice is being ignored in practice, or the volume of
decisions makes a lighter-weight log preferable.
