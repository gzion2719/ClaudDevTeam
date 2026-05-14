# ROADMAP.md

**Guiding principle:** We are not in a rush. Milestones end when they actually work — not when the calendar says so. A milestone is done when its acceptance check passes, not when its tasks are checked off.

---

## Milestone 1 — Handoff Contract

**Goal:** Define and document the shared file format that all three role prompts will use to pass context between sessions. This is the prerequisite for everything else.

**Tasks:**
- [ ] Complete ADR-0001: handoff contract format (file names, required fields, optional fields, version field) — *Draft; H1 title rule added 2026-05-14; promotes to Active after Milestone 3 test loop*
- [x] Create `docs/handoff/` directory with sample skeleton files for each role output — *2026-05-14*
- [x] Review ADR-0001 against all 5 non-negotiables — confirm no gaps — *2026-05-14; gaps G1–G3 logged in BACKLOG*

**Acceptance check:** ADR-0001 is complete, sample handoff files exist, and a human reading only the ADR + samples could write a compliant handoff file without asking a question.

---

## Milestone 2 — Three Role Prompts

**Goal:** Write `roles/Architect.md`, `roles/Developer.md`, and `roles/QA.md` — each a self-contained bootstrap prompt in the style of `Developer.md`, adapted for its role.

**Tasks:**
- [x] Write `roles/Architect.md` (system design, produces `ARCH_BRIEF.md`) — *2026-05-14; draft, validated by Milestone 3 test loop*
- [ ] Write `roles/Developer.md` (implementation, reads `ARCH_BRIEF.md`, produces `DEV_SUMMARY.md`)
- [ ] Write `roles/QA.md` (quality validation, reads `DEV_SUMMARY.md`, produces `QA_REPORT.md`)
- [ ] Each prompt: cold-start mode + handoff-intake mode both documented
- [ ] Each prompt: opening ritual detects entry mode automatically

**Acceptance check:** Each role prompt can be pasted into a fresh Claude chat and produces the correct handoff file without additional explanation.

---

## Milestone 3 — Full Loop Test

**Goal:** Run a synthetic mini-project through the complete Architect → Developer → QA → Architect cycle and validate that handoffs work end-to-end.

**Tasks:**
- [ ] Define the synthetic mini-project scenario (≤2h, produces one file per role)
- [ ] Run Architect session → produce `ARCH_BRIEF.md`
- [ ] Run Developer session reading `ARCH_BRIEF.md` → produce `DEV_SUMMARY.md`
- [ ] Run QA session reading `DEV_SUMMARY.md` → produce `QA_REPORT.md`
- [ ] Run Architect revision session reading `QA_REPORT.md` → confirm loop closes
- [ ] Document failures and fix before marking done

**Acceptance check:** All four sessions complete, all handoff files are produced, the revision-cycle Architect session correctly reads QA output without any manual context-setting by the user.

---

## Milestone 4 — Refine and Extend

**Goal:** Tighten the prompts based on Milestone 3 findings. Prepare the system for adding new roles.

**Tasks:**
- [ ] Process all BACKLOG items surfaced during Milestones 1-3
- [ ] Document the "add a new role" pattern (what to copy, what to adapt, what ADR to amend)
- [ ] Consider: PM / Product role above Architect? DevOps / Infra role after QA?
- [ ] Improve the original `Developer.md` based on lessons learned

**Acceptance check:** A new team member reading only `README.md` + `docs/AGENTS.md` + the three role files can add a new role without asking a question.
