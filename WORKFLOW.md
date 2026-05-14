# WORKFLOW.md

**Language:** Any language in, English out.

How chats work across the ClaudeDevTeam project.

---

## Chat Archetypes

### 1. Design
Focus: handoff contract, ADRs, role prompt architecture.
Starter prompt: `"Let's work on the handoff contract / ADR-0001 / role prompt design."`

### 2. Build
Focus: writing or editing a role prompt file (`roles/Architect.md`, `roles/Developer.md`, `roles/QA.md`).
Starter prompt: `"Let's build / refine the [Architect / Developer / QA] prompt."`

### 3. Test
Focus: running a mini-project through one or more role prompts to validate them.
Starter prompt: `"Let's run a test loop — [describe the synthetic mini-project scenario]."`

### 4. Research / Unrelated
Focus: exploration, questions, adjacent topics not tied to a deliverable.
Starter prompt: `"Quick question about [topic] — not a build session."`

---

## Fresh-Chat Triggers

Start a new chat when:
- ~30 exchanges in the current chat
- Topic switches between archetypes (e.g., Design → Test)
- Phase boundary (milestone completed)
- Claude repeats a corrected mistake or contradicts an earlier decision in the same chat

---

## End-of-Session Phrase

Any of these triggers the closing ritual:
`"תודה על היום"` / `"see you tomorrow"` / `"we're done"` / `"let's call it"` / `"thanks"` / goodbye emoji / any closing phrase

---

## Git Flow

Two long-lived branches: `main` (production / deploy target) and `dev` (integration). Every change goes on a short-lived branch off `dev` and is merged back through TWO pull requests.

```
feature/<topic>  ──PR #1──▶  dev  ──PR #2──▶  main  ──▶  deploy
```

**Step 1 — Open a feature branch off `dev`:**

```bash
git fetch origin
git checkout -b feature/<short-topic> origin/dev
# ... make changes ...
git add <files>
git commit -m "<type>: <description>"
git push -u origin feature/<short-topic>
gh pr create --base dev   # PR #1: feature → dev
```

**Step 2 — After PR #1 merges, promote `dev` → `main`:**

```bash
gh pr create --base main --head dev --title "promote: <what's in this batch>"
```

PR #2 (`dev` → main) is opened whenever a coherent batch of feature PRs is ready to ship. It is NOT opened per feature — multiple feature PRs can land in `dev` first, then a single promotion PR brings them to `main`.

**Step 3 — `main` triggers deploy.**

Deploy target is TBD (see `docs/BACKLOG.md` under "Tooling & Automation"). When hosting is added, a deploy workflow runs on push to `main`. Until then, `main` is the release-ready snapshot.

**Branch naming:** `feature/`, `fix/`, `docs/`, `chore/` prefixes. Never commit directly to `main` or `dev`.

---

## CI Gate (local mirror of CI)

**One-time setup** (run once after cloning):
```bash
npm install -g markdownlint-cli
```

Before every push, run:
```bash
markdownlint "**/*.md" --ignore node_modules
```

This mirrors what `.github/workflows/ci.yml` runs on push. Running it locally catches red CI in seconds rather than waiting for the pipeline.

---

## Red Flags

Stop and flag to the user if:
- Claude repeats a mistake that was corrected earlier in the same chat
- Claude contradicts a decision recorded in an ADR or CLAUDE.md
- A role prompt is being written before ADR-0001 is complete (violates non-negotiable #1)
- A "test" session produces no concrete file output (doesn't count per non-negotiable #4)
- A handoff file is being read by a role prompt that hasn't checked which entry mode it's in (violates non-negotiable #5)

---

## Handoff Flow (the load-bearing mechanic)

```
[Architect session]
  └─ produces: docs/handoff/ARCH_BRIEF.md
       └─ [Developer session reads ARCH_BRIEF.md]
            └─ produces: docs/handoff/DEV_SUMMARY.md
                 └─ [QA session reads DEV_SUMMARY.md]
                      └─ produces: docs/handoff/QA_REPORT.md
                           └─ [Architect session reads QA_REPORT.md → revision cycle]
```

The exact file names and fields are defined in ADR-0001. This diagram is the shape; ADR-0001 is the contract.

---

## Emergency Protocol

If a role prompt produces nonsensical or contradictory output during a test loop:
1. Stop the test — do not continue the loop with bad output.
2. Record the failure mode in `docs/BACKLOG.md` under "Open Questions".
3. Open a Design session to diagnose before re-testing.
