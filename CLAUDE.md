# CLAUDE.md

**Project:** ClaudeDevTeam — a multi-role Claude prompt system (Architect / Developer / QA) with shared handoff rituals.
**User:** galzi.
**Language:** Any language in, English out.

---

## On first message of any chat

Read `SESSION_PROTOCOL.md` and `WORKFLOW.md` immediately. Do not proceed until both are in context.

---

## Non-negotiables

1. **Handoff contract first.** No role prompt is written before the shared handoff file format is defined and recorded in ADR-0001.
2. **Full loop before "done".** A role prompt is not considered complete until the full Architect → Developer → QA → back cycle has been tested end-to-end with a real mini-project.
3. **Prompts are portable.** Every role prompt must work standalone — no dependency on a specific Claude client (Cowork, Claude Code, web, API). If a feature requires a specific client, it is opt-in and documented.
4. **Test before ship.** Every prompt change must be validated by running a mini-project through the affected role. A mini-project is a synthetic scenario that takes ≤2 hours and produces one concrete file output per role (architecture brief / implementation summary / QA report). A session that produces no file output does not count as a test.
5. **Cold-start vs. handoff detection.** Every role prompt's opening ritual must detect which entry mode it is in — (a) new project (no handoff file present) or (b) revision cycle (handoff file from prior role present) — and branch accordingly.

---

## Continuous improvement north star

Every chat must leave the project measurably better on two axes: (1) the quality of the role prompts and handoff artifacts, and (2) how-we-work efficiency (friction reduced, rules tightened, ritual steps improved). This applies fractally — to a single rule edit as much as to a full milestone. It is codified in the opening ritual's Step 7 (planning self-critique) and the closing ritual's Step 1 (retrospective). If a session ends without a process improvement bullet, that is a signal, not a pass.

---

## Style rules

- Build incrementally — no role prompt ships before its prerequisite (ADR-0001 → role prompts → full-loop test).
- Ask when uncertain via `AskUserQuestion`. Do not guess at scope.
- Capture scope creep into `docs/BACKLOG.md`, not into the current session.
- Ground answers in repo docs, not generic advice.
- **Always follow `WORKFLOW.md`'s Git Flow.** Re-read the Git Flow section before any `git push` or `gh pr create` — do not rely on a cached version. Never commit directly to `main` or `dev`. PRs targeting `main` are promotion-only; feature PRs always target `dev`.

---

## File map

```
ClaudeDevTeam/
├── CLAUDE.md                        ← this file (always read first)
├── SESSION_PROTOCOL.md              ← opening + closing ritual
├── WORKFLOW.md                      ← chat archetypes + red flags
├── CHATLOG.md                       ← session memory, newest-first
├── README.md                        ← project vision + mutual agreement
├── Developer.md                     ← original Developer bootstrap prompt (source material)
├── .gitignore
├── .github/
│   └── workflows/
│       └── ci.yml                   ← markdownlint on push
├── roles/
│   ├── Architect.md                 ← (Milestone 2)
│   ├── Developer.md                 ← (Milestone 2)
│   └── QA.md                        ← (Milestone 2)
└── docs/
    ├── ROADMAP.md
    ├── BACKLOG.md
    ├── AGENTS.md                    ← role responsibilities + interfaces
    ├── CHATLOG_ARCHIVE.md           ← (created at session 10)
    └── adr/
        ├── README.md                ← ADR index
        └── 0001-handoff-contract-format.md
```
