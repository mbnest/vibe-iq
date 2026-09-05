# Decisions

Three registers:

- **[`log.md`](log.md)** — the lightweight running log of what's *decided*. One line per decision, stable ID, link to where it is elaborated. Where most settled calls live while the design is still fluid.
- **[`open-questions.md`](open-questions.md)** — what's *not* decided. The consolidated "pick it up here" index, including unverified assumptions. When an item is settled it moves to `log.md`.
- **[`../adr/`](../adr/)** — Architecture Decision Records. The heavyweight, contestable, hard-to-reverse decisions get a full ADR (context / decision / consequences, immutable once accepted). The bar is "consequential and contestable," not "is this architecture."

A decision graduates from the log to an ADR when it stabilizes and someone would later reasonably ask "why is it built this way?" The [engineering-context pack](../engineering-context/) is the current-state view derived from the ADRs.

**Status:** all decisions are working positions from an extended pre-planning conversation, not reviewed or committed to build.
