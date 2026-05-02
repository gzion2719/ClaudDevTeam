# AGENTS.md

The three roles in the ClaudeDevTeam system. Each role is a self-contained Claude session persona — a prompt file under `roles/` that a user pastes into a fresh chat to activate that role.

---

## Role: Architect

**File:** `roles/Architect.md`
**Entry modes:** Cold-start (new project) | Handoff-intake (reads `QA_REPORT.md` for revision cycle)

**Responsibilities:**
- Define system architecture: components, interfaces, data flows, technology choices
- Document non-negotiables and hard constraints for the implementation
- Produce `ARCH_BRIEF.md` — the handoff artifact the Developer reads

**Produces:** `docs/handoff/ARCH_BRIEF.md`
**Reads (revision cycle):** `docs/handoff/QA_REPORT.md`

**Non-negotiables for this role:**
- Every architecture decision that affects an interface must be recorded (not just described)
- The brief must be self-contained — Developer should need no other context to begin implementation
- Ambiguities must be resolved in the brief, not left for the Developer to guess

---

## Role: Developer

**File:** `roles/Developer.md`
**Entry modes:** Cold-start (no prior architecture) | Handoff-intake (reads `ARCH_BRIEF.md`)

**Responsibilities:**
- Implement according to the architecture brief
- Document implementation decisions, deviations from the brief, and open questions
- Produce `DEV_SUMMARY.md` — the handoff artifact the QA engineer reads

**Produces:** `docs/handoff/DEV_SUMMARY.md`
**Reads:** `docs/handoff/ARCH_BRIEF.md`

**Non-negotiables for this role:**
- Any deviation from the architecture brief must be documented in `DEV_SUMMARY.md` with rationale
- The summary must include a "known gaps / not implemented" section for QA awareness
- The Developer prompt is the evolution of `Developer.md` in the project root — lessons from this system feed back into it

---

## Role: QA Engineer

**File:** `roles/QA.md`
**Entry modes:** Cold-start (independent review) | Handoff-intake (reads `DEV_SUMMARY.md`)

**Responsibilities:**
- Validate the implementation against the architecture brief and stated requirements
- Identify defects, gaps, and risks
- Produce `QA_REPORT.md` — the handoff artifact the Architect reads for the revision cycle

**Produces:** `docs/handoff/QA_REPORT.md`
**Reads:** `docs/handoff/DEV_SUMMARY.md` (and optionally `docs/handoff/ARCH_BRIEF.md`)

**Non-negotiables for this role:**
- Every finding must be classified: critical (blocks) / major (should fix) / minor (nice to fix)
- The report must include an explicit go/no-go recommendation for the revision cycle
- QA does not fix — it reports. Fixes are the Architect/Developer's job in the next cycle.

---

## Handoff Chain Summary

```
Architect  →  ARCH_BRIEF.md  →  Developer
Developer  →  DEV_SUMMARY.md →  QA Engineer
QA Engineer → QA_REPORT.md  →  Architect (revision)
```

The exact required fields for each file are defined in ADR-0001.

---

## Extending the System

To add a new role:
1. Define its place in the handoff chain (what it reads, what it produces)
2. Amend ADR-0001 with the new handoff file format
3. Write `roles/<RoleName>.md` following the same structure as existing role files
4. Add the role here in `AGENTS.md`
5. Run a mini-project test through the updated chain before considering it done
