# BACKLOG.md

Scope-creep capture and deferred items. Reviewed every 5 sessions (CHATLOG entry count divisible by 5).

---

## Git Workflow

- [ ] Add branch protection to `main` on GitHub (Settings → Branches → require PR before merge)
- [ ] Define git flow in WORKFLOW.md: `feature/<topic>` branches → PR → merge to main
- [ ] Add git flow question to bootstrap interview Stage 1 Round 3 (currently missing from Developer.md)

---

## Bootstrap Prompt Improvements (Developer.md)

- [ ] Add write-time reminder to Stage 3: before closing any generated file, confirm language convention is stated identically to CLAUDE.md. Currently only caught by Stage 4 self-verify — should be a generation-time habit.

---

## Handoff Contract & ADRs

- [ ] Define versioning scheme for handoff file format (e.g., `schema_version: "1.0"` field) so future role additions can detect incompatible formats
- [ ] Consider: should handoff files be JSON/YAML for machine-readability, or stay `.md` for human-readability? (Currently assumed `.md` — revisit if automation is added)
- [ ] **G1 — handoff location for self-tests:** ADR-0001 says handoff files live in the *target project's* `docs/handoff/`, but our own Milestone 3 test loop runs inside ClaudeDevTeam. Decide: do self-test handoffs live in this repo's `docs/handoff/` (current skeletons), in a sandbox subfolder, or in a separate temp repo? Surfaces during Milestone 3 setup.
- [ ] **G2 — schema_version mismatch behavior:** ADR-0001 declares the field but doesn't specify what a role should do when reading a file with a different version. Defer to v1.1 of the contract — only matters once we have a v1.1.
- [ ] **G3 — `project:` metadata format:** undefined (slug? free text?). Skeletons use free text. Tighten if/when automation reads this field.

---

## Role Prompts

- [ ] PM / Product Manager role (above Architect) — deferred from bootstrap
- [ ] DevOps / Infra role (after QA) — deferred from bootstrap
- [ ] "Improve the existing Developer.md" — the original prompt in the project root; lessons from building the new role prompts should feed back into it

---

## Tooling & Automation

- [ ] Consider a simple shell script (`scripts/new-project.sh`) that creates the `docs/handoff/` directory and blank handoff file stubs for a new project using the ClaudeDevTeam system
- [ ] Consider `cspell` addition to CI once role prompts are stable (currently minimal CI = markdownlint only)

---

## Open Questions

- What is the right granularity for the QA report? (Bug list? Test coverage assessment? Both?)
- Should the Architect prompt support multiple architecture styles (e.g., monolith vs. microservices) or be style-agnostic?
- How should the Developer prompt handle cases where the architecture brief is ambiguous or incomplete — ask for clarification or proceed with documented assumptions?
