# ADR-0003: Allow SQLite for local and low-volume workloads

## Status

Accepted - 2026-09-03

Extends [ADR-0002](0002-target-application-stack.md) (target application stack).

## Context

ADR-0002 sets PostgreSQL as the primary datastore. In practice a lot of early
work - local development, test suites, demos, and small single-node workloads -
does not need a running Postgres server, and requiring one adds friction and
infrastructure for no benefit at that scale. SQLite covers these cases with zero
operational overhead and is well supported by the chosen stack (SQLAlchemy,
Alembic).

The risk is scope creep: SQLite quietly becoming the store for something that
later needs concurrent writers or more than one application instance, at which
point it violates the "services own their data" and stateless-application-tier
principles.

## Decision

We will allow SQLite at ring **Trial**, scoped to:

- local development and automated tests,
- demos and throwaway prototypes,
- single-node, low-volume production workloads where there is one writer.

It must not be used for any multi-instance service. Schemas are kept
Postgres-compatible (types, no SQLite-only features) so a workload can graduate
to Postgres without a rewrite.

## Consequences

- Faster local setup and CI; no Postgres container needed for most tests.
- One more datastore in the codebase, and a boundary that has to be watched at
  design review - "does this need more than one writer or instance?"
- Keeping schemas portable is a small ongoing tax on SQLite-backed code.
- `allowed-stack.md` carries the scope note; design review enforces the line.

**Revisit when:** a SQLite-backed demo or workload needs to become a real
multi-user or multi-instance service, or the portability constraint starts
causing friction.
