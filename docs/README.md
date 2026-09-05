# Documentation

**Start here → [`overview.md`](overview.md)** — the whole platform in one read (~10 min), with links out to detail.

This folder is also published as a site via GitHub Pages ([`index.md`](index.md) is the front door). To enable: repo **Settings → Pages → Source: Deploy from a branch → `main` / `/docs`**. Local preview: `cd docs && bundle install && bundle exec jekyll serve`.

## Map

| Directory | What's in it | Authority |
|---|---|---|
| [`overview.md`](overview.md) | The high-level read; the entry point for everyone | Current |
| [`product-vision.md`](product-vision.md) | The executive "so what": product pitch, per-feature business value, suggested build-out order | Current |
| [`architecture/`](architecture/) | How the platform is built: process flow, front-of-flow, data model, runtime | Current design position |
| [`agents/`](agents/) | One spec per agent and human gate | Current design position |
| [`policies/`](policies/) | Cross-cutting rules: autonomy tiering, requirement-change handling, governance vs. business rules | Current design position |
| [`decisions/`](decisions/) | Decision log (what's decided), open-questions index (what isn't), pointer to ADRs | Current |
| [`adr/`](adr/) | Architecture Decision Records — the heavyweight, immutable ones | Authoritative |
| [`engineering-context/`](engineering-context/) | The standing rules the pipeline's agents read at each stage (for the *products it builds*) | Authoritative (starter content) |
| [`examples/`](examples/) | A real ask (`govern-iq`) run through the artifact chain as a worked example | Fixture |
| [`notes/`](notes/) | The working journal — the pre-planning conversations. History, not spec | Superseded by the above |

## Reading orders

- **New to the project:** `overview.md` → skim `architecture/process-flow.md` → pick one agent that interests you.
- **Evaluating the design:** `overview.md` → `decisions/log.md` → `decisions/open-questions.md` → `architecture/front-of-flow.md`.
- **Picking up the work:** `decisions/open-questions.md` → the consolidated index.
- **Building the MVP:** `notes/architecture-napkin.md` → `architecture/runtime.md` → `agents/intake-agent.md` + `agents/design-agent.md`.

## Status

Nothing here is reviewed or committed to build. It is the output of an extended pre-planning conversation. Treat every agent assignment, autonomy tier, and decision as a current working position to challenge.
