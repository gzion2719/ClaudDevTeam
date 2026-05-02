# ADR-0001 — Handoff Contract File Format

**Status:** Draft (needs validation in Milestone 3 test loop)
**Date:** 2026-05-02
**Deciders:** galzi + Claude

---

## Context

The ClaudeDevTeam system has three role prompts (Architect, Developer, QA) that operate in separate Claude chat sessions. Each session produces a handoff artifact that the next role reads. Without a defined contract, each role would need manual context-setting by the user, defeating the purpose of the handoff ritual.

This ADR defines: (1) the file names, (2) the required and optional fields for each handoff file, (3) the schema version field for future evolution, and (4) the cold-start vs. handoff-intake detection rule.

---

## Decision

### File locations

All handoff files live in `docs/handoff/` relative to the project root being worked on by the ClaudeDevTeam system. (Note: this is the *target project's* root, not the ClaudeDevTeam repo itself.)

```
<target-project>/
└── docs/
    └── handoff/
        ├── ARCH_BRIEF.md
        ├── DEV_SUMMARY.md
        └── QA_REPORT.md
```

### Schema version

Every handoff file begins with a metadata block:

```markdown
---
schema_version: "1.0"
role: <Architect | Developer | QA>
date: YYYY-MM-DD
project: <project name>
---
```

### ARCH_BRIEF.md — required fields

```markdown
---
schema_version: "1.0"
role: Architect
date: YYYY-MM-DD
project: <project name>
---

## System Overview
<1-3 sentence description of what is being built>

## Components
<list of named components with one-line responsibility each>

## Interfaces
<list of interfaces between components: who calls what, data shape>

## Technology Choices
<language, frameworks, key libraries — with rationale if non-obvious>

## Non-Negotiables
<hard constraints the Developer must not deviate from>

## Open Questions / Assumptions
<anything unresolved that the Developer should flag if it affects implementation>
```

### DEV_SUMMARY.md — required fields

```markdown
---
schema_version: "1.0"
role: Developer
date: YYYY-MM-DD
project: <project name>
---

## What Was Built
<1-3 sentence summary of the implementation>

## Deviations from Architecture Brief
<any departure from ARCH_BRIEF.md — required even if "none">

## Known Gaps / Not Implemented
<explicit list of what was scoped out — required even if "none">

## Key Implementation Decisions
<non-obvious choices made during implementation with rationale>

## How to Run / Verify
<commands or steps to see the implementation working>
```

### QA_REPORT.md — required fields

```markdown
---
schema_version: "1.0"
role: QA
date: YYYY-MM-DD
project: <project name>
---

## Go / No-Go
<Go | No-Go> — <one sentence rationale>

## Findings

### Critical (blocks revision cycle)
<list — or "none">

### Major (should fix)
<list — or "none">

### Minor (nice to fix)
<list — or "none">

## Validation Performed
<what was actually checked — tests run, docs reviewed, interfaces verified>

## Recommended Next Steps for Architect
<specific, actionable — not generic advice>
```

### Cold-start vs. handoff-intake detection rule

Every role prompt's opening ritual MUST check for the presence of the relevant handoff file in `docs/handoff/`:

| Role | Handoff file to check | If absent → | If present → |
|------|----------------------|-------------|--------------|
| Architect | `QA_REPORT.md` | Cold-start mode: run project definition interview | Revision-cycle mode: read QA_REPORT.md, skip interview |
| Developer | `ARCH_BRIEF.md` | Cold-start mode: ask user for architecture context | Handoff-intake mode: read ARCH_BRIEF.md, confirm understanding before proceeding |
| QA | `DEV_SUMMARY.md` | Cold-start mode: ask user for implementation context | Handoff-intake mode: read DEV_SUMMARY.md + optionally ARCH_BRIEF.md |

---

## Consequences

- All three role prompts depend on this contract. Any change to required fields requires amending this ADR and updating the affected role files.
- The `schema_version` field allows future role additions to detect incompatible formats and surface a clear error rather than silently misreading a file.
- Status moves from Draft → Active after Milestone 3 test loop completes successfully.
