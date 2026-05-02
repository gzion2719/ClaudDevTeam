# New Project Bootstrap Prompt

> **What this is:** a single block to paste into the FIRST message of a new Claude chat when starting a new project. It tells Claude to run a discovery interview, generate a YuTom-style protocol scaffold (CLAUDE.md / SESSION_PROTOCOL.md / WORKFLOW.md / CHATLOG.md / docs/ROADMAP.md / docs/BACKLOG.md / README.md), self-verify, and hand off — all before any real project work begins.
>
> **How to use it:** open a new Claude chat (Cowork, Claude Code, web, doesn't matter), mount or create the new project folder, then paste everything inside the `--- BEGIN PROMPT ---` / `--- END PROMPT ---` block below as your first message. Claude will take it from there.
>
> **Lineage:** this prompt is a portable extraction of the YuTom Trading Bot session protocol — same shape (opening/closing rituals, just-in-time context, ADR discipline, scope-sprawl audit, continuous improvement north star), parameterized content.

---

## --- BEGIN PROMPT ---

I am starting a new project. Before any project work, you will bootstrap a protocol-driven workflow scaffold for it — same shape as my YuTom Trading Bot project (opening/closing rituals, just-in-time context loading, ADR discipline, scope-sprawl audit, continuous improvement as a load-bearing principle), parameterized to this new project's domain.

**Language convention for THIS bootstrap session:** I write to you in Hebrew; you answer in English. The scaffolded project's language pair is decided in Stage 1 — each new project picks its own and that pair becomes non-negotiable for every future session of THAT project (recorded in its `CLAUDE.md`). If I don't pick during the interview, default to "my-input-language in, English out".

**Process discipline for THIS bootstrap session:** you will run six stages in order. Do NOT skip stages. Do NOT generate files before Stage 3. Surface uncertainty via `AskUserQuestion`; do not guess.

---

### Stage 1 — Project definition interview (THE most important stage; all processes + automations are decided here)

This is the load-bearing stage of the bootstrap. Every process and automation the project will use — language pair, opening/closing rituals, hygiene cadence, ADR discipline, git workflow, CI/CD, pre-push gate — gets pinned in this interview. Get it right here and the rest of the bootstrap is mechanical; get it wrong here and every future session pays the cost.

Run THREE rounds of `AskUserQuestion` in order. Don't skip ahead. If a later answer makes an earlier one inconsistent, flag it and re-ask. Each question offers concrete options where possible; "Other" is always available for free-text.

**Round 1 — Identity & setup** (4 questions):
1. **Project name** — short, hyphen- or space-separated, max 30 chars. Becomes the folder name and the identity sentence in CLAUDE.md.
2. **Project domain** — software / writing / research / business or operations / personal system / mixed / other.
3. **Primary deliverable shape** — code repository / document set / decision log / mixed / other. Drives whether ADR directory, pre-push gate, and CI/CD make sense.
4. **Working folder on my Mac** — give me the absolute path you want the project to live at. If the folder doesn't exist yet (the common case for a brand-new project), CREATE it first via bash (`mkdir -p "<path>"`) and then mount it via `mcp__cowork__request_cowork_directory` (or the client equivalent). If the folder already exists, just mount it. **Do not proceed past Round 1 until you can read and write files inside this folder** — if the mount fails or the path is unreachable, surface the error and re-ask. The scaffold lands inside this folder; if you can't write here, the next four stages can't run.

**Round 2 — Interaction & constraints** (4 questions):
5. **What language should I (Claude) answer in?** — English / the language I (the user) write to you in / Other (specify). The user's input language is whatever language I type in; this question is purely about your output. Recorded in CLAUDE.md as the project's permanent language convention. Default if I skip: my-input-language in, English out.
6. **Phasing rhythm** — week-by-week with acceptance checks (YuTom style) / milestone-by-milestone (less granular) / open-ended (no pre-committed phases). Drives ROADMAP.md shape.
7. **Hard non-negotiables** — free text (or short list). What MUST happen and what MUST NOT happen. Examples from YuTom: "paper trading 60+ days before live money", "total capital cap 500 NIS", "every position has a stop-loss". For non-financial projects: audience constraints, accuracy bars, deadline gates, ethics rules. If I'm not sure, propose 2-3 candidates from the project domain and let me edit.
8. **Internal roles beyond me + you?** — for YuTom this was "the agents inside the system" (Data Collector, Risk Manager, …). For a writing project: "narrator / fact-checker / editor". For a research project: "lit-reviewer / experimenter / writer". Skip if N/A — most projects don't need this.

**Round 3 — Automations** (4 questions, with proposed defaults derived from Rounds 1-2):

Before asking, derive the proposed CI/CD defaults from Rounds 1-2's domain + deliverable answers, so I'm picking from grounded options rather than naming tools from scratch. Map:
- **Software (Python)** → ruff + black --check + mypy --strict + pytest --cov on push to main and on every PR (the YuTom mirror).
- **Software (Node/TypeScript)** → eslint + prettier --check + tsc --noEmit + vitest/jest on push and PR.
- **Software (other languages)** → equivalent lint + format-check + types + tests; pick concrete tools for the language and name them.
- **Writing / docs** → markdownlint + lychee link-check + cspell on push.
- **Research / data** → notebook execution check (nbqa or papermill) + dataset-hash validation if applicable.
- **Personal system / PKM** → minimal: a smoke job confirming the repo builds / files are well-formed.

Then ask:

9. **Git remote target — MANDATORY, no local-only.** GitHub / Bitbucket / Other (specify). Default = GitHub. Every scaffolded project gets pushed to a remote; this is non-negotiable.
10. **New repo or existing?** — new (you'll give me the exact `gh repo create` / `bb repo create` command at handoff) / existing (you'll give me the `git remote add` + first-push commands at handoff).
11. **CI/CD posture** — Full CI as proposed above for my domain (show the concrete tool list before asking) / Minimal CI (one smoke job only) / Manual-only (no workflow file; document in WORKFLOW.md) / None.
12. **Hygiene cadence** — every 5 sessions BACKLOG review + every 10 CHATLOG archival (YuTom default) / different cadence (specify) / none.

After all three rounds, restate the full picture in 6-10 bullets covering identity / language pair / phasing / non-negotiables / roles / git remote / CI shape / hygiene cadence, and explicitly ask "go?" Wait for confirmation before Stage 2. Do not proceed without explicit go.

---

### Stage 2 — Pre-generation self-critique (Step-7 style)

Before writing any files, run an explicit critique on the answers and the planned scaffold. Surface, at minimum:

- **Internal consistency** — does any answer contradict another? E.g., "open-ended phasing" + "weekly acceptance checks" is incoherent; pick one. "Minimal CI" + non-negotiable that includes type-safety is incoherent; flag it.
- **Missing-rule audit** — does the non-negotiable list have a domain-implied gap? Examples: a financial project without a loss cap; a writing project without an audience definition; a research project without a confidence-claim policy. Propose the missing rule explicitly.
- **Scaffold scope** — is the FULL set (CLAUDE / PROTOCOL / WORKFLOW / CHATLOG / ROADMAP / BACKLOG / README + optional ADR + AGENTS) right-sized for this project, or is a smaller scaffold cleaner? For tiny one-week projects, CHATLOG + ROADMAP alone might be the right starter; ADR / AGENTS / pre-push gate get added when the project earns them. Default = full scaffold; downsize only if the project is provably small.
- **Automation fit** — does the chosen CI/CD posture (Round 3 #11) match the non-negotiables and the deliverable shape? E.g., "decision-log" deliverable that picked "Full CI" is over-engineered; flag it. "Code repository" that picked "None" but has type-safety as a non-negotiable is dangerous; flag it. Surface the mismatch and propose the correction; ask before changing.
- **Continuous improvement adapter** — every project's protocol needs the closing-ritual retrospective + the in-session-edit-the-rule mechanism. For non-code projects, the "edit the rule in this session" target is still SESSION_PROTOCOL.md. Confirm this lands.

Show the critique findings as a short list. Propose adjustments. Wait for "go" before Stage 3.

---

### Stage 3 — File generation

Generate the following files inside the working folder. Substitute placeholders from Stage 1. Where the YuTom shape is reused verbatim, reuse it; where it's parameterized, parameterize it.

**CLAUDE.md** (the always-read-first file):
- Identity sentence: "Project: <name> — <one-line description>. User: <user>. Language: <user-language-in> in, <claude-language-out> out."
- Reference: read `SESSION_PROTOCOL.md` and `WORKFLOW.md` immediately on first message of any chat.
- Non-negotiables list (from Round 2 #7 + any rules added in Stage 2 critique).
- Continuous improvement north star paragraph: every chat must leave the project measurably better on two axes (project-specific output quality + how-we-work efficiency); applies fractally; codified in opening Step 7 + closing Step 1.
- Style rules: build incrementally, ask when uncertain, scope-creep capture into `docs/BACKLOG.md`, ground answers in repo docs not generic advice.
- File map of what was actually generated.

**SESSION_PROTOCOL.md** (the strict ritual file):
- Trigger statement: opening ritual fires on the FIRST user message of any chat — full stop, no magic word.
- Opening ritual steps 1-7:
  1. Greet warmly, one line.
  2. Verify the working folder is mounted (path should end with `<project-name>/`). If NOT mounted, request it via `mcp__cowork__request_cowork_directory` (or the client equivalent) BEFORE any other action — file reads will fail without it. Confirm to the user "Folder confirmed: ✅" before proceeding. This step is the self-healing fallback when the user opens a new chat without pre-mounting.
  3. Confirm WORKFLOW.md is in effect (read it if not already).
  4. Just-in-time orient: read ONLY `CHATLOG.md` last 3 entries + `docs/ROADMAP.md` upfront. Defer deeper docs (RISK_MANAGEMENT, AGENTS, ARCHITECTURE, TECH_STACK if any) to Step 4b after Step 6 focus is chosen. The principle: load just-in-time, not just-in-case.
  5. Git status (if code project) or working-state summary (if not) — flag drift.
  6. Ask for current focus via `AskUserQuestion` with 2-3 grounded options. Include the **scope-sprawl audit sub-rule**: if the previous CHATLOG entry's "Next session:" line bundles ≥3 distinct deliverables OR introduces an ADR-worthy architectural surface, present the smaller-cleaner first increment as the Recommended option.
  7. Restate the chosen focus, run a substantive planning self-critique (is this the most performant + efficient approach? are iron rules covered? is there a smaller cleaner first increment? are there missed verification paths?), wait for "go". For trivial focuses, a one-line ack is fine; for new content landing in user-facing docs / code modules / architectural decisions, the critique must be a substantive list — not theater.
- Closing ritual triggered by farewell phrase ("תודה על היום" / "see you tomorrow" / any closing): use the **verbatim content in "Closing Ritual — Verbatim Text for SESSION_PROTOCOL.md" below** as the closing-ritual section of SESSION_PROTOCOL.md. Substitute project-specific terms (`<project root>`, `<pre-push gate command>`, language pair from Stage 1 Round 2 #5) inline; do not paraphrase the structure or drop the worked example.
- Recurring hygiene rituals:
  - Every 5 sessions (CHATLOG entry count divisible by 5): backlog review — read `docs/BACKLOG.md`, surface 1-2 ripe items as Step 6 options.
  - Every 10 sessions: CHATLOG archival — keep the most recent 5 entries active in CHATLOG.md, move older routine entries to `docs/CHATLOG_ARCHIVE.md` newest-first; keep entries with decisions / non-obvious learnings / gotchas in place. When in doubt, keep.
  - Sandbox git reads use `git --no-optional-locks` (if code project) — prevents stale `.git/index.lock`.
  - Pre-push gate (if code project, define what runs in `make pre-push`).
- ADR discipline (only if the project domain warrants ADRs — most software projects, some research projects, rarely writing projects):
  - **ADR-with-new-types sub-rule:** ADRs that introduce frozen dataclasses with structural invariants get a 10-line type skeleton inline in the ADR's Decision section before the prose.
  - **Test-helper-signature sub-rule:** ADRs that prescribe a test-helper / fixture signature change include the new signature as a code skeleton in the Decision section, not just a prose description.

---

#### Closing Ritual — Verbatim Text for SESSION_PROTOCOL.md

The block between the BEGIN/END markers below is the verbatim closing-ritual content to copy into SESSION_PROTOCOL.md as its closing-ritual section. Substitute project-specific terms inline before saving: `<project root>` → the working folder path from Stage 1 Round 1 #4; `<pre-push gate command>` → the project's gate command (e.g., `make pre-push` for Python software, `npm run check` for Node, etc., per Stage 1 Round 3 #11 + WORKFLOW.md); `<language pair>` and recap-language references defer to CLAUDE.md's pinned convention from Stage 1 Round 2 #5. Do not paraphrase the structure, drop the worked example, or strip the philosophy paragraph — those are load-bearing.

--- BEGIN CLOSING RITUAL VERBATIM TEXT ---

~~~markdown
## Closing Ritual

**When to run.** Triggered by ANY farewell phrase from the user — "תודה על היום", "see you tomorrow", "we're done", "let's call it", "thanks", a goodbye emoji, anything that signals end-of-session. Don't just say goodbye; run the ritual.

**Why it exists.** The closing ritual is NOT a session diary. It exists to make the NEXT session's first 60 seconds frictionless: read the last 3 entries, know exactly where we left off and where to look for detail. The orientation chain reads it every chat. Each entry's job is "where we left off, what the open question is, where to look for the detail." Compounding is the whole game — one concrete improvement per session × 200 sessions = a system that runs perfectly with zero friction.

### Step 1 — Retrospective (the most important step)

Before writing anything for the record, take a structured look at the SESSION ITSELF — not the work product. Three bullets, in your head or on screen:

- **What worked:** moves that were efficient, decisions that paid off, friction we successfully avoided.
- **What didn't:** protocol slips, dead ends, things we redid, places we read/wrote/checked things we didn't need, over-engineered fixes.
- **Improvement for next session:** ONE concrete, actionable change. A protocol tweak, a habit shift, a new rule of thumb.

The improvement is the OUTPUT, and it has two possible homes:

1. **Codifiable as a rule** (it almost always is) — edit the relevant file IN THIS SAME SESSION (`SESSION_PROTOCOL.md`, `CLAUDE.md`, `WORKFLOW.md`, an ADR, etc.). The edit IS the improvement; don't write a separate description of it. **Before editing, do a conflict check:** grep the file for related rules, confirm the new wording doesn't contradict anything already there.
2. **Not yet codifiable** (an observation we want to remember but can't yet generalize) — keep it as the CHATLOG bullet only.

Either way, ALWAYS add a `**Process improvement:**` bullet to the CHATLOG entry. If genuinely none, say `none this session` explicitly — never silently skip. Future-Claude needs to know we looked.

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

Constraints — enforced, not aspirational:

- **Max 5 content bullets** plus the two trailing ones (`Process improvement` + `Next session`). 7 lines total under the date header. If the session genuinely produced more than 5 distinct points, pick the 5 most useful for next session's orientation.
- **Each bullet ≤ 2 sentences.** If a bullet wants to be 4 sentences, the second half belongs in a rule file, an ADR, or BACKLOG — not the CHATLOG.
- **`Process improvement` is a 1-line pointer**, not a retelling. The file edit IS the improvement; the bullet exists to make it discoverable. Format: `Rule X gained Y sub-rule (see <file>)` or `ADR-NNNN written, see <path>`.
- **Don't re-tell bug stories that live elsewhere.** If a bug birthed a sub-rule, the rule file has the imperative + concrete trigger + date pointer; the CHATLOG entry has at most one sentence on what was caught and where the rule lives.
- **No "compounding scoreboard" / meta-reflection bullets.** Reflective meta-content about which codifications fired is closing-ritual reflection value (Step 1), not next-session orientation value (Step 2). Think it once during retrospective, then don't write it.

### Step 3 — Write the entry to CHATLOG.md

Insert the new entry directly below the header / `---` separator, before any existing dated entries (newest-first ordering). Show the user the entry you wrote.

### Step 4 — Report uncommitted work

For code projects, run `git --no-optional-locks status` from the sandbox (the `--no-optional-locks` flag prevents stale `.git/index.lock` per the standing rule). For non-code projects, summarize the working-state delta: which docs/files changed, what's saved vs unsaved, what's in the project's deliverable folder.

List the changed/new files and suggest a commit message.

### Step 5 — Give the exact commands the user needs (gate-first)

Don't assume the user will remember. The handoff message LEADS with the gate-first bash block — first content block, before any prose summary, before any file list, before any closing-trigger nudge.

For code projects:

```
cd "<project root>"
<pre-push gate command>           # the project's CI mirror — see WORKFLOW.md / Makefile
git add <files>
git commit -m "<suggested commit message>"
git push
```

The gate is a verbatim mirror of the project's CI job (defined per Stage 1 Round 3 #11). Running it locally before pushing catches red CI in seconds rather than minutes-plus-roundtrip; that's why it leads.

For non-code projects, replace the gate with the project's finalization checklist (e.g., spell-check + link-check + version bump for a writing project; export + share for an operations project) — same gate-first ordering applies.

**Mechanical pre-send self-check.** Re-read your draft's first 3 lines before sending. If they don't contain the gate command (or the equivalent finalization step), the draft is wrong — prepend it. The "summary first, gate last" pattern is a recurring anti-pattern; the gate goes first because that's what the user runs first. This applies to ANY "ready to commit" handoff, not just the closing one — including ADR-only commits, protocol-only commits, anything that gets pushed. The check is text-based, not memory-based; apply it to your own draft mechanically.

### Step 6 — Close warmly

One line. Match the user's register and the language pair pinned in CLAUDE.md (e.g., if CLAUDE.md says "Hebrew in / English out", the farewell is in English).

### Step 7 — Plain-English recap + concrete example

AFTER Step 6's farewell, append to the SAME message a short paragraph (3-5 sentences max) summarizing what you did this session in plain language, followed by ONE concrete example that makes the work tangible — a command we ran, a behavior we built, a decision we made. The CHATLOG bullets cover technical depth; this is the human anchor. Keep it simple, no jargon dumps. Format:

```
---
**In plain English:**
<3-5 short sentences about what we did and why>

**Example:**
<one concrete example — a command, a behavior, a decision, or a snippet>
```

The recap uses the project's standard language pair as defined in CLAUDE.md — the same one the rest of the chat uses. **History note:** in some past projects this section was granted a one-time bilingual exception (e.g., recap in user's input language even when the chat was English-out). If your project's CLAUDE.md doesn't explicitly grant that exception, don't carry it over — stay consistent with the pinned pair. The exception caused readability issues in past projects when the rendering didn't match user-side font support; default to the standard pair.

### Worked example

(Example from the YuTom Trading Bot project — a Python multi-agent crypto trading bot with `CHATLOG.md`, `SESSION_PROTOCOL.md`, ADRs under `docs/adr/`, `make pre-push` as the gate command, and a Hebrew-in / English-out language pair. Substitute your project's equivalents.)

User says: "thanks, that's enough for today."

You produce:

> **Step 1 — Retrospective.** Worked: parallel reads in the orientation phase saved ~3 round-trips; the type-skeleton-before-Decision rule caught a contradiction in the ADR before code landed. Didn't: I named "Path A vs Path B" as the load-bearing decision but hadn't yet read the existing types — the critique couldn't actually evaluate which. The right answer landed by lucky general reasoning. Improvement: codify "for ADR/design focus, do the supporting reads BEFORE the planning self-critique, not after" — small edit to SESSION_PROTOCOL.md.
>
> **Step 2 — Entry:**
>
> ```
> ## 2026-04-29 — ADR-0009 typed event payloads (Slice B3 prerequisite)
> - Wrote ADR-0009 (~630 LOC) deciding typed event payloads for the Strategy↔RiskManager bus handoff: minimal wrapper dataclasses + Event.payload widening to object + new event_topics.py registry as canonical topic→type map.
> - Step 7 critique caught the load-bearing question pre-code: Path A (reuse Signal as payload) fails completeness; picked Path B (minimal wrappers). Step 6 scope-sprawl audit deferred wrapper architecture to ADR-0010.
> - ADR-with-new-types + test-helper-signature sub-rules both fired with skeletons inline. Sandbox Rule 5: mypy --strict + runtime smoke clean.
> - Cross-ref grep clean: ADR-0008 + ADR-0007 forward-references already point to ADR-0009 correctly.
> - **Process improvement:** SESSION_PROTOCOL.md Step 4b gained the **ADR/design-document timing sub-rule** (see SESSION_PROTOCOL.md Step 4b).
> - **Next session:** ADR-0010 — bus-citizen wrapper architecture.
> ```
>
> **Step 3 — Inserted** into CHATLOG.md below the `---` separator, before the most recent dated entry.
>
> **Step 4 — Uncommitted:** `CHATLOG.md` (+10 lines), `SESSION_PROTOCOL.md` (+2 lines).
>
> **Step 5 — Commands:**
>
> ```
> cd "~/Desktop/AI Tools/YuTom Trading Bot"
> make pre-push
> git add CHATLOG.md SESSION_PROTOCOL.md
> git commit -m "docs(protocol): log ADR-0009 + Step 4b ADR/design-document timing sub-rule"
> git push
> ```
>
> **Step 6 — נתראה מחר / See you tomorrow.**
>
> ---
> **In plain English:** Today we wrote the design document for how two parts of the system will talk to each other over an event bus, using strongly-typed messages instead of untyped dictionaries. We also tightened the protocol itself: when the focus is a design document, the supporting reads now happen before the planning critique, not after — that way the critique can actually evaluate the design instead of just naming the questions.
>
> **Example:** A handler now receives `Event(payload=SignalGenerated(symbol="BTC/USDT", signal=Signal(...), strategy_name="ema"))` and narrows it with `assert isinstance(event.payload, SignalGenerated)` — typed end-to-end, mypy --strict honest, no untyped dict reaches into handler bodies.
~~~

--- END CLOSING RITUAL VERBATIM TEXT ---

---

**WORKFLOW.md** (how chats work across the project):
- Three chat archetypes: Build (or domain-equivalent: Write, Research, Operate, …) / Research / Unrelated.
- Starter prompt for each chat type — short, paste-ready.
- Fresh-chat triggers: ~30 exchanges, topic switch, phase boundary, Claude forgetting/contradicting.
- End-of-session phrase that triggers the closing ritual.
- Pre-push gate (if code project) or finalization equivalent.
- Red flags: Claude repeats corrected mistakes, contradicts an earlier decision in the same chat, generates content contradicting docs.
- Emergency protocol if applicable to the domain (e.g., "trip the kill switch" for the trading bot; "freeze the draft" for a publication project).

**CHATLOG.md** (session memory, newest-first):
- Header + how-to-use instructions.
- First dated entry summarizing the bootstrap session itself, in the standard format (max 5 content bullets + `**Process improvement:**` + `**Next session:**`).

**docs/ROADMAP.md** (phased plan):
- Guiding principle (paraphrasing YuTom's "we are not in a rush" — phases end when they actually work, not on a calendar).
- Phases shaped by Stage 1 #6 answer.
- Each phase: goal sentence + checklist + acceptance check.

**docs/BACKLOG.md** (skeleton):
- Categories appropriate to the project domain (YuTom uses Architecture & Docs / Strategies / Risk / Tooling / Open Questions; adapt).

**docs/AGENTS.md** (only if Round 2 #8 listed roles):
- One section per role, with responsibilities + interfaces + non-negotiables.

**docs/adr/0001-<slug>.md** + **docs/adr/README.md** index (only if a structural decision was made during bootstrap, OR if the project domain warrants ADR discipline going forward):
- ADR-0001 documents the bootstrap-time decisions (file structure, language convention, phasing rhythm) so the precedent exists.

**README.md** (project vision):
- Vision sentence + non-negotiables + structure + mutual agreement section (what Claude commits to, what user commits to).
- Identifies the git remote (Round 3 #9) and the CI/CD posture (Round 3 #11) so future contributors and future-me can see the workflow at a glance.

**.gitignore** — always generated. Sensible defaults for the language(s) and OS (`.DS_Store`, editor folders). For software projects, language-appropriate (Python: `__pycache__/`, `.venv/`, etc.; Node: `node_modules/`, etc.).

**.github/workflows/ci.yml** OR **bitbucket-pipelines.yml** (based on Round 3 #9 + #11):
- **Full CI for Python software** (the YuTom mirror): trigger on `push` to main + `pull_request`; jobs run `ruff check` → `black --check` → `mypy --strict` → `pytest --cov`. Cache the venv. Python version pinned (default 3.12 if not specified in non-negotiables).
- **Full CI for Node/TypeScript**: trigger on push + PR; `pnpm install` (or `npm ci`) → `eslint .` → `prettier --check .` → `tsc --noEmit` → `vitest run` (or `jest`).
- **Full CI for writing/docs**: `markdownlint **/*.md` + `lychee --no-progress **/*.md` + `cspell **/*.md` on push.
- **Full CI for research**: `nbqa black --check notebooks/` + `nbqa flake8 notebooks/` + papermill smoke-execution on a tiny dataset.
- **Minimal CI**: one job that runs the most basic well-formed-repo check (e.g., `python -c "import ast; [ast.parse(open(f).read()) for f in glob('**/*.py', recursive=True)]"` for Python; `markdownlint **/*.md` for docs).
- **Manual-only or None**: don't generate the workflow file; instead document in WORKFLOW.md that CI is run manually with the specific commands, and explain why CI is off.

**Makefile** (only if software project AND CI is not "None"): standard targets — `install`, `install-dev`, `test`, `test-fast`, `lint`, `format`, `ci`, `pre-push`, `precommit-install`, `help`. Same shape as YuTom's. The `pre-push` target is a verbatim mirror of what `.github/workflows/ci.yml` runs, so the local gate matches CI exactly.

**.pre-commit-config.yaml** (only if software project AND CI is "Full"): commit-stage hooks for lint+format auto-fix; pre-push hook running `make pre-push`. Auto-installed by `make precommit-install`.

---

### Stage 4 — Self-verify the scaffold

After every file is written, read each one back and check:

- CLAUDE.md correctly references `SESSION_PROTOCOL.md` + `WORKFLOW.md` by exact filename.
- The file map at the bottom of SESSION_PROTOCOL.md or CLAUDE.md matches the files actually generated (no phantom files, no missing files).
- ROADMAP.md phases align with the user's phasing-rhythm answer in Round 2 #6.
- The opening-ritual instructions don't reference any path or filename that doesn't exist in the scaffold.
- Non-negotiables in CLAUDE.md and README.md match.
- Continuous improvement is referenced in BOTH opening Step 7 (planning leg) AND closing Step 1 (retrospective leg).
- Language convention is stated identically across CLAUDE.md / SESSION_PROTOCOL.md / WORKFLOW.md.

If any drift, fix it before handoff. Do not hand off a self-inconsistent scaffold.

---

### Stage 5 — Handoff

Report, in this order, with the git/remote commands LEADING the message (gate-first ordering — same posture as the YuTom Rule 6 pre-send self-check):

1. **Git initialization + remote setup commands** — paste the exact `bash` block I should run on my Mac, derived from Round 3 #9 + #10:

   - **New GitHub repo** (preferred default):
     ```bash
     cd "<working folder path>"
     git init
     git add .
     git commit -m "chore: bootstrap project scaffold"
     gh repo create <project-name> --private --source=. --remote=origin --push
     ```
     (Requires `gh` CLI authenticated. If not installed, fall back to creating the repo on github.com manually, then `git remote add origin <url>` + `git push -u origin main`.)

   - **New Bitbucket repo**:
     ```bash
     cd "<working folder path>"
     git init
     git add .
     git commit -m "chore: bootstrap project scaffold"
     # Create the repo at bitbucket.org first (CLI option only if `bb` is installed)
     git remote add origin git@bitbucket.org:<workspace>/<project-name>.git
     git push -u origin main
     ```

   - **Existing remote**:
     ```bash
     cd "<working folder path>"
     git init
     git add .
     git commit -m "chore: bootstrap project scaffold"
     git remote add origin <existing remote url>
     git push -u origin main
     ```

   If CI is "Full", remind me to run `make precommit-install` after the first push so the pre-push gate is wired up locally going forward.

2. **The exact files created**, with `computer://` links for each. Group by directory.
3. **3-line summary** of what's now in place — what the scaffold encodes, what CI will run, what the first session will look like.
4. **How to open the next chat** — explain to me, in two sentences, the exact mount-then-trigger flow:
   - When I open a new chat (Cowork / Claude Code / web), mount the project folder FIRST: in Cowork that's "Select folder" → pick `<working folder path>`; in Claude Code that's `cd "<working folder path>"` before launching; in web Claude that's an upload-folder action if available.
   - Then send the trigger phrase below. The generated SESSION_PROTOCOL.md's Step 2 also requests the folder via `mcp__cowork__request_cowork_directory` if I forgot to pre-mount, so the ritual is self-healing — but pre-mounting saves a round trip.
5. **The trigger phrase** to start the FIRST real session in this project (e.g., "שלום חבר" if I write in Hebrew, "hi" if I write in English) — that one phrase fires the opening ritual the scaffold defines, no other ceremony required.
6. **First-session focus suggestion** — typically Phase 1 / Week 1 / first task on the ROADMAP. Optional but useful.

---

### Stage 6 — Close THIS session

The bootstrap session is itself a session. Close it via the closing ritual the scaffold just wrote — write a CHATLOG.md entry for the bootstrap session (the first dated entry was a placeholder you wrote in Stage 3; replace it with a real retrospective entry now), report uncommitted work, give the pre-push commands if applicable, close warmly, plain-English recap.

This dogfooding step is the test that the scaffold works on day one. If anything in the closing ritual feels awkward, that's a real signal — fix it in `SESSION_PROTOCOL.md` before closing, the same way the YuTom project tightens its rules in-session.

---

### Constraints across all six stages

- **Self-critique is non-negotiable.** Every stage that produces user-facing content (Stages 2, 3, 4, 5, 6) ends with an explicit critique pass. This is the load-bearing mechanism for continuous improvement; if you skip it, the scaffold inherits the protocol's discipline only nominally.
- **Continuous improvement is the north star.** The scaffold you generate must encode this in BOTH the opening ritual (planning leg) AND the closing ritual (retrospective leg). It's not a footer — it's the spine.
- **Stage 1 is the most important stage.** Every process and automation the project will use is decided there. If Stage 1 is rushed, every future session pays. Take the rounds at full depth even if it feels slow — that slowness is the whole point.
- **Git remote is non-negotiable.** Every scaffolded project gets pushed to GitHub or Bitbucket — local-only repos are not an option. The Stage 5 handoff includes the exact remote-setup commands; the user runs them on their Mac because the sandbox can't perform git writes.
- **CI/CD is proposed by domain, picked by user.** Don't ask the user to name tools from scratch — derive concrete defaults from Round 1's domain + deliverable answers and present them as options in Round 3 #11. The user picks the depth (Full / Minimal / Manual / None); the tools come from the proposal.
- **Just-in-time over just-in-case.** Don't pre-load docs the scaffold doesn't yet contain. Don't write speculative ADRs. Generate what Stage 1 said is needed and nothing more.
- **Ask, don't guess.** If a Stage 1 answer is ambiguous, re-ask via `AskUserQuestion`. If a Stage 2 critique surfaces a question I haven't answered, ask before generating.
- **No magic words.** The scaffold's opening ritual fires on the FIRST message of any chat — period. Greeting, question, work request, emoji-only, one-word — all trigger the ritual. There is no list of magic words.
- **Folder-mount is the first-message prerequisite, with self-healing.** Step 2 of the generated opening ritual verifies the working folder is mounted; if not, it requests the mount via `mcp__cowork__request_cowork_directory` before any other action. This means the user can open a new chat without pre-mounting and the ritual still works — the pre-mount in Stage 5's "How to open the next chat" guidance is an optimization, not a requirement.
- **Fallback for chats without file tools.** If you're running in a Claude UI that lacks file Write tools, generate every Stage 3 file as a clearly-labeled markdown code block in chat, instruct me how to save them manually (one block per file, with the target path as the block's heading), and skip Stage 6's "write CHATLOG entry to disk" step — replace it with "here's the entry to paste into CHATLOG.md after you save the files." The git remote setup commands at Stage 5 are unaffected — they run on the user's Mac regardless of Claude's tool surface.

---

### Begin

Start at Stage 1. Use `AskUserQuestion` for the discovery interview. Don't just describe what you would do — actually do it.

## --- END PROMPT ---

---

## Design notes

**Why AskUserQuestion instead of `[placeholder]` syntax:** placeholders let you skip thinking; the AskUserQuestion approach forces explicit decisions upfront — which is exactly what the YuTom Step-6 + Step-7 discipline does at session level. The bootstrap mirrors the same discipline at project level. If you ever want a fast-path placeholder version (e.g., for spinning up many similar tiny projects), I can produce one — but the interview-driven version is the right default.

**Why Stage 1 is the heart of the prompt:** you said "the first part is the most important — that's where all processes and automations are defined." The three-round structure (identity → interaction & constraints → automations) is built around that emphasis. If a future-Claude rushes Stage 1, every future session of that new project pays the cost. Round 3 specifically is non-skippable: language pair, git remote, CI/CD, hygiene cadence — these are the load-bearing automation choices.

**Why git remote is mandatory and not optional:** local-only repos lose history to disk failures, OS reinstalls, and folder-renames-gone-wrong. Pushing to GitHub or Bitbucket gives you a free off-site backup, a permission model, a place for CI to attach to, and a future-collaboration path. The 30 seconds it takes to `gh repo create` pays back the first time anything goes wrong. The bootstrap therefore refuses to scaffold a local-only project.

**Why CI/CD is proposed by domain rather than asked from scratch:** users don't always know what tools they want, and a blank "what CI tools?" question produces either tool-soup (everything everyone heard of) or nothing (decision paralysis). Round 3 #11 hands the user concrete defaults derived from Round 1's domain answer (Python software → ruff+black+mypy+pytest; writing → markdownlint+lychee+cspell; etc.), and asks only for the depth (Full / Minimal / Manual / None). Picking depth is faster and cleaner than picking tools.

**Why the scaffold dogfoods its own closing ritual at Stage 6:** if the protocol is broken on day one, you'll find out before any real work is on the line. This is the same posture as YuTom's own first session: the kill switch was the first code we wrote, not the last.

**Where this file lives:** this `prompt.md` is the canonical bootstrap-protocol content for the `new-project-bootstrap` Claude Code plugin. It lives at `skills/new-project-bootstrap/prompt.md` inside the plugin repo (published at `tomzion90/new-project-bootstrap` on GitHub; cloned locally to wherever the maintainer keeps their plugin repos). Edit it here, commit, push — installed users get the update via `/plugin update new-project-bootstrap`. The skill's `SKILL.md` reads this file via the relative path `./prompt.md`, which works because the plugin's runtime context is its own folder regardless of which Cowork project the user has mounted.

**Tweaks you might want before first use:**
- Round 2 question 8 (internal roles) is YuTom-shaped — for non-multi-agent projects it's usually skipped. The prompt already says "skip if N/A".
- Stage 3's ADR section says "only if the project domain warrants ADRs" — if you want ADRs by default for every project, strip that conditional.
- Stage 5's git block assumes `gh` CLI for GitHub. If you don't have it installed, the prompt already includes the manual-web fallback. Same posture for Bitbucket.
- The Round 3 #11 CI defaults are opinionated (Python = the YuTom mirror). If you want different defaults per language/domain, edit the mapping table inside Round 3 directly.
