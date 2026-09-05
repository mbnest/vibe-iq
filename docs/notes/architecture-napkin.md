# Architecture — Back of Napkin

*Working sketch, 2026-09-04. Major components of the platform on one page, with the MVP scope highlighted against the full system. Deliberately rough — it collapses detail that the other thinking docs carry. Companion to [`platform-notes.md`](platform-notes.md); the rigorous process view is in [`process-flow.md`](../architecture/process-flow.md).*

## Diagram

```mermaid
flowchart TB
    classDef mvp fill:#dbeafe,stroke:#1d4ed8,stroke-width:3px,color:#1e3a8a
    classDef later fill:#f4f4f5,stroke:#a1a1aa,stroke-dasharray:4 3,color:#52525b
    classDef ext fill:#faf5ff,stroke:#c084fc,stroke-dasharray:4 3,color:#6b21a8

    subgraph FRONT["FRONT OF FLOW — per initiative  (build later)"]
        direction TB
        SIG[Signal] --> SCO[Scoping]
        SCO --> PF[/"Product function<br/>ROI · prioritize · backlog"/]
        PF --> BB["Business brief (L1)"]
        BB --> PD["Product definition (L2)<br/>validated with business"]
        PD --> SOLA["Solution analysis<br/>agent-drafted"]
        SOLA --> SSO{{"Solution sign-off<br/>human"}}
        SSO --> STG["Feature / story generation"]
        STG --> BKLG[("Backlog — Jira / GitHub")]
    end

    subgraph BUILD["PER-STORY BUILD LOOP"]
        direction TB
        INT["Intake agent"] --> SPEC["spec.md"]
        SPEC --> RSO{{"Requirements sign-off<br/>human · change-request loop"}}
        RSO --> DSN["Design agent"] --> DDOC["design.md"]
        DDOC --> PLN["Planning / breakdown"]
        PLN --> IMP["Implementation"] --> TST["Testing"] --> REV["Review"]
        REV --> MRA{{"Merge / release approval<br/>human"}}
        MRA --> ENB["Enablement<br/>catalog / novel path"]
        ENB --> DPL["Deployment"] --> VLD["Post-release validation"]
    end

    subgraph OPS["OPS TAIL — not yet scoped"]
        OPSX["Incident · Maintenance · Deprecation"]
    end

    subgraph SUB["PLATFORM SUBSTRATE — underpins every stage"]
        direction TB
        LLM["LLM<br/>judgment inside steps"]
        ORC["Orchestration<br/>durable · deterministic"]
        ARTS[("Artifact store<br/>versioned handoffs + validation")]
    end

    subgraph GOV["GOVERNANCE & CONTEXT"]
        direction TB
        ECP["Engineering context pack<br/>stack · principles · constraints<br/>· standards · glossary"]
        ADRL["ADR log"]
        GDL["Governance decision logic<br/>autonomy tiering · gap routing<br/>· change classification"]
        BRR["Business rule registry"]
    end

    subgraph EXT["EXTERNAL DEPENDENCIES — existence unconfirmed"]
        direction TB
        MCP["Brownfield context<br/>MCP over codebases"]
        SIB["Sibling enablers<br/>Terraform · testing · data quality · CI/CD"]
    end

    BKLG --> INT
    VLD --> OPSX
    OPSX -.new asks.-> SCO
    MCP -.-> INT
    MCP -.-> IMP
    ENB -.novel path.-> SIB
    ECP --> INT
    ECP --> DSN
    GDL --> INT
    GDL --> DSN
    LLM --> INT
    LLM --> DSN

    class INT,SPEC,DSN,DDOC,RSO,ECP,ADRL,LLM mvp
    class SIG,SCO,PF,BB,PD,SOLA,SSO,STG,BKLG,PLN,IMP,TST,REV,MRA,ENB,DPL,VLD,OPSX,ORC,ARTS,GDL,BRR later
    class MCP,SIB ext
```

## Reading guide

**Blue, bold = MVP.** Intake agent → `spec.md` → requirements sign-off (change-request loop, no direct human edits) → Design agent → `design.md`. Plus the LLM doing the judgment, the engineering-context pack it reads, and the ADR log. Single reviewer, greenfield, GitHub issue in / docs out. Runs as a skill or prompt-loop.

**Grey, dashed = full system, built later.** The whole front of flow (v0.5), the rest of the build loop (planning through validation), the ops tail, and the heavier substrate — durable orchestration and a real artifact store rather than git files.

**Purple, dashed = external, existence unconfirmed.** Brownfield MCP context and the four sibling enablers. Every one is a row in the External Dependency Status table that still needs checking against reality.

**Nuances the colour can't show:**
- `Governance decision logic` and `Artifact store` exist in the MVP in a minimal form — pure functions for the first, git for the second — behind a clean interface, to be swapped for real components later ([`rules-and-policy.md`](../policies/rules-and-policy.md)).
- The MVP Design agent already does consumer-side validation of `spec.md` on input — the first instance of handoff validation.
- `Product function` may be a human or an agent; it is largely outside platform scope either way.

*Status: sketch, 2026-09-04. Regenerate when the front-of-flow fan-out or the substrate choices firm up.*
