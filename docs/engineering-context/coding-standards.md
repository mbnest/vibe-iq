# Coding Standards

*Code-craft expectations. Read by the Implementation and Review stages. **Formatters and linters are authoritative for mechanical style** — this document covers only the judgment calls tooling cannot make.*

*Placeholder set — confirm and adjust. Tool configs referenced here live in the repo root / `pyproject.toml` / `.eslintrc` and are versioned alongside this file.*

## Tooling is the first authority

| Concern | Python | TypeScript |
|---|---|---|
| Formatting | Ruff format | Prettier |
| Linting | Ruff | ESLint |
| Types | Pyright (strict) | tsc (strict) |
| Tests | pytest | Vitest |

If tooling and this document disagree on mechanical style, tooling wins and this document is wrong — fix the document.

## Naming

- Names reveal intent. A reader should not need the implementation to know what a thing is for.
- Domain terms match [`glossary.md`](glossary.md) and the spec's data model. If the spec says "subscriber," the code says `subscriber`, not `user` or `customer`.
- Avoid abbreviations except well-known ones (`id`, `url`, `db`).

## Functions and modules

- A function does one thing. If describing it needs an "and," split it.
- Prefer short modules with a single clear responsibility over large utility grab-bags.
- Keep nesting shallow — early returns over deep `if` pyramids.

## Error handling

- Catch an exception only where you can do something useful about it. Otherwise let it propagate.
- Never swallow an exception silently. If you catch and continue, log why.
- Fail fast on programmer error (bad arguments, impossible state). Handle expected failures (network, validation) explicitly.
- *(Open decision: exceptions vs. result types for expected failures. Resolve and record as an ADR.)*

## Tests

- Every behavior described by a spec acceptance criterion has a test that maps to it.
- Test behavior, not implementation — a refactor that preserves behavior should not break tests.
- Shape: many fast unit tests, fewer integration tests, a thin layer of end-to-end. Do not invert it.
- A good test: one reason to fail, a name that says what it checks, no dependence on another test's side effects.

## Comments

- Comment *why*, not *what*. The code says what.
- A docstring on every public function and class: what it is for, not a restatement of the signature.
- Delete commented-out code. Git remembers it.

## Dependencies

- Adding a dependency is a decision. Prefer the standard library and libraries already present.
- Anything outside [`allowed-stack.md`](allowed-stack.md) needs a recorded exception.

## Golden examples

*(To be added: links to three to five exemplar files in this repo — a well-formed service, a data model, a test suite — once they exist. Placeholder until then.)*

*Status: starter pack, 2026-09-03. Placeholder pending confirmation; error-handling convention is an open decision.*
