# ClaudeDevTeam

A multi-role Claude prompt system that lets you run a software project through a full Architect → Developer → QA team — each role operating in its own Claude chat session, passing structured handoff artifacts between sessions.

---

## Vision

Most Claude-assisted development is single-role: you describe what you want and Claude builds it. ClaudeDevTeam adds discipline: the Architect thinks before the Developer builds, and the QA validates before anything is considered done. Each role has a prompt, a ritual, and a handoff artifact. The loop closes.

---

## Non-Negotiables

1. **Handoff contract first.** No role prompt is written before ADR-0001 is complete.
2. **Full loop before "done".** Every role prompt is tested through a complete Architect→Dev→QA→back cycle.
3. **Prompts are portable.** No dependency on a specific Claude client.
4. **Test before ship.** Every change validated by a mini-project (≤2h, concrete file output per role).
5. **Cold-start vs. handoff detection.** Every role prompt detects its entry mode automatically.

---

## Structure

```
roles/
  Architect.md     ← system design, produces ARCH_BRIEF.md
  Developer.md     ← implementation, produces DEV_SUMMARY.md
  QA.md            ← validation, produces QA_REPORT.md

docs/
  ROADMAP.md       ← 4 milestones
  BACKLOG.md       ← deferred items + open questions
  AGENTS.md        ← role responsibilities + interfaces
  handoff/         ← sample handoff file skeletons
  adr/
    0001-handoff-contract-format.md
```

---

## How to Use

1. Open a new Claude chat
2. Paste the contents of the relevant role file (`roles/Architect.md`, `roles/Developer.md`, or `roles/QA.md`) as your first message
3. The role's opening ritual fires automatically — no magic word needed
4. At the end of the session, say any farewell phrase to trigger the closing ritual and produce the handoff file

---

## Git Remote

GitHub — private repo. See Stage 5 handoff for `gh repo create` command.

## CI/CD

Minimal — `markdownlint "**/*.md"` on push to main and on pull requests. See `.github/workflows/ci.yml`.

---

## Mutual Agreement

**Claude commits to:**
- Following the SESSION_PROTOCOL.md opening and closing rituals without exception
- Never writing a role prompt before ADR-0001 is complete
- Detecting cold-start vs. handoff-intake mode at the start of every role session
- Capturing scope creep into BACKLOG.md, not into the current session

**User commits to:**
- Running `markdownlint "**/*.md"` before every push
- Validating every prompt change with a mini-project test
- Saying a farewell phrase at session end to trigger the closing ritual
- Keeping ADR-0001 up to date when handoff fields change
