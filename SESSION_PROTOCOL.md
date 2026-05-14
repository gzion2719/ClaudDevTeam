# SESSION_PROTOCOL.md

**Language:** Any language in, English out.

The opening ritual fires on the **FIRST user message of any chat — full stop**. No magic word required. Greeting, question, emoji-only, one word — all trigger the ritual.

---

## Opening Ritual

### Step 1 — Greet
Greet warmly, one line.

### Step 2 — Verify working folder
Confirm the working folder is mounted and ends with `ClaudeDevTeam/`. If NOT mounted, request it via the client's folder-mount mechanism (`mcp__cowork__request_cowork_directory` in Cowork; `cd` in Claude Code) BEFORE any other action. Confirm to the user **"Folder confirmed: ✅"** before proceeding.

### Step 3 — Confirm WORKFLOW.md is in effect
Read `WORKFLOW.md` if not already in context.

### Step 4 — Just-in-time orient
Read ONLY `CHATLOG.md` last 3 entries + `docs/ROADMAP.md` upfront. Defer `docs/AGENTS.md`, `docs/adr/`, and role files to Step 4b after focus is chosen. Load just-in-time, not just-in-case.

### Step 5 — Working-state summary
Summarize which files changed since the last session (git status if available, or file-mod-time scan). Flag any drift from the last CHATLOG entry's "Next session:" line.

### Step 6 — Ask for current focus
Ask via `AskUserQuestion` with 2-3 grounded options derived from the ROADMAP and last CHATLOG entry.

**Scope-sprawl audit sub-rule:** If the previous CHATLOG entry's "Next session:" line bundles ≥3 distinct deliverables OR introduces an ADR-worthy decision surface, present the smaller-cleaner first increment as the Recommended option.

### Step 4b — Load role-specific context (after Step 6 focus is chosen)
If the focus involves a specific role prompt (Architect / Developer / QA), now read `docs/AGENTS.md` and the relevant role file under `roles/` (if it exists). Also read any referenced ADRs. Defer all others.

### Step 7 — Planning self-critique
Restate the chosen focus. Run a substantive self-critique: Is this the most efficient approach? Are all non-negotiables covered? Is there a smaller cleaner first increment? Are there missed verification paths? For trivial focuses a one-line ack is fine; for anything landing in role prompts, handoff contract, or ADRs the critique must be a substantive list — not theater.

Wait for **"go"** before proceeding.

---

## Closing Ritual

**When to run.** Triggered by ANY farewell phrase — "תודה על היום", "see you tomorrow", "we're done", "let's call it", "thanks", a goodbye emoji, anything that signals end-of-session. Don't just say goodbye; run the ritual.

**Why it exists.** The closing ritual is NOT a session diary. It exists to make the NEXT session's first 60 seconds frictionless: read the last 3 entries, know exactly where we left off and where to look for detail. Each entry's job is "where we left off, what the open question is, where to look for the detail."

### Step 1 — Retrospective (the most important step)

Before writing anything for the record, take a structured look at the session itself — not the work product. Three bullets:

- **What worked:** moves that were efficient, decisions that paid off, friction we successfully avoided.
- **What didn't:** protocol slips, dead ends, things we redid, over-engineered fixes.
- **Improvement for next session:** ONE concrete, actionable change.

The improvement has two possible homes:

1. **Codifiable as a rule** — edit the relevant file IN THIS SAME SESSION (`SESSION_PROTOCOL.md`, `CLAUDE.md`, `WORKFLOW.md`, an ADR). The edit IS the improvement. Before editing, grep the file for related rules to confirm no contradiction.
2. **Not yet codifiable** — keep it as the CHATLOG bullet only.

Always add a `**Process improvement:**` bullet to the CHATLOG entry. If genuinely none, say `none this session` explicitly — never silently skip.

Show the user the proposed improvement (and any file edits) before moving on. They approve or refine.

### Step 2 — Generate the CHATLOG entry

Compose a 3-5 bullet summary in this exact format:

```
## YYYY-MM-DD — <session title>
- <What we did, bullet 1>
- <What we did, bullet 2>
- <Key decision or learning>
- <Any blockers or open questions>
- **Process improvement:** <what we changed and which file, OR "none this session">
- **Next session:** <one sentence on what's first>
```

Constraints:
- **Max 5 content bullets** + the two trailing ones. 7 lines total under the date header.
- **Each bullet ≤ 2 sentences.**
- **`Process improvement` is a 1-line pointer**, not a retelling.
- **Don't re-tell bug stories that live elsewhere.** If a decision birthed a rule, the rule file has the detail; CHATLOG has one sentence pointing there.
- **No meta-reflection bullets** about which codifications fired.

### Step 3 — Write the entry to CHATLOG.md

Insert the new entry directly below the header / `---` separator, before any existing dated entries (newest-first). Show the user the entry you wrote.

### Step 4 — Report working-state delta

Summarize which docs/files changed, what's saved, what's in the `roles/` folder vs. what the ROADMAP says should exist. Suggest a commit message.

### Step 5 — Give the exact commands (gate-first)

The handoff message LEADS with the gate-first block — before any prose, before any file list.

**Before drafting this block, Read `WORKFLOW.md`'s Git Flow section.** Do not rely on remembered flow. Confirm the current branch model and PR base. As of 2026-05-14 the flow is `feature → PR (base: dev) → dev → PR (base: main) → main → deploy`. Feature PRs target `dev`, not `main`.

```bash
cd "C:\Users\galzi\OneDrive - Afiki-C\Development\Claude\ClaudeDevTeam"
markdownlint "**/*.md"           # CI mirror — catch lint before push
git add <files>
git commit -m "<suggested commit message>"
git push -u origin <branch>
gh pr create --base <dev|main>   # base per WORKFLOW.md, NOT defaulted
```

**Mechanical pre-send self-check.** Re-read your draft's first 3 lines before sending. If they don't contain the gate command, prepend it. The gate goes first because that's what the user runs first. Also verify the `--base` flag is present and matches WORKFLOW.md — never let `gh pr create` default the base.

### Step 6 — Close warmly

One line, in English (the project's output language per CLAUDE.md).

### Step 7 — Plain-English recap + concrete example

After Step 6's farewell, append to the SAME message:

```
---
**In plain English:**
<3-5 short sentences about what we did and why>

**Example:**
<one concrete example — a file we created, a decision we made, a handoff flow we tested>
```

---

## Recurring Hygiene Rituals

- **Every 5 sessions** (CHATLOG entry count divisible by 5): BACKLOG review — read `docs/BACKLOG.md`, surface 1-2 ripe items as Step 6 options.
- **Every 10 sessions**: CHATLOG archival — keep the most recent 5 entries active in `CHATLOG.md`, move older routine entries to `docs/CHATLOG_ARCHIVE.md` newest-first. Keep entries with decisions, non-obvious learnings, or gotchas in place. When in doubt, keep.

---

## ADR Discipline

ADRs live in `docs/adr/`. Write one when a structural decision is made that future role additions or prompt changes will need to reference.

- **ADR-with-handoff-contract sub-rule:** ADRs that define a handoff file format include a sample file skeleton inline in the Decision section — not just prose description.
- **Cold-start vs. handoff sub-rule:** ADRs that modify a role prompt's opening behavior must include both entry-mode branches (cold-start and handoff-intake) in the Decision section as a side-by-side comparison.
