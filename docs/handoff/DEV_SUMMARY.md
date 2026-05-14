---
schema_version: "1.0"
role: Developer
date: YYYY-MM-DD
project: <project name or slug>
---

<!--
SKELETON — sample handoff file conforming to ADR-0001.

Replace each section's placeholder text with real content.
Do NOT delete section headers — every required section must remain present,
even if its body is "none" or "not applicable".

This file is read by the QA role in handoff-intake mode.
-->

# DEV_SUMMARY — <project name>

## What Was Built

<1-3 sentence summary of the implementation that actually shipped.>

Example:
> Implemented `csvconvert` per ARCH_BRIEF.md. Streams the source CSV row-by-row,
> applies YAML-defined transforms, writes the target CSV. CLI entry point at
> `python -m csvconvert --in <path> --out <path> --map <path>`.

## Deviations from Architecture Brief

<Any departure from ARCH_BRIEF.md. Required even if "none".>

Example:
- `RowMapper.transform` returns `Row | list[Row]` instead of `Row | None` —
  one source row can produce multiple target rows in the new mapping rules.
  Brief assumed 1:1; spec'd by user mid-implementation.

## Known Gaps / Not Implemented

<Explicit list of what was scoped out. Required even if "none".>

Example:
- No retry logic on write failure — caller's scheduler handles retry
- No progress bar — logs row count every 10k rows instead

## Key Implementation Decisions

<Non-obvious choices made during implementation, with rationale.>

Example:
- Used `csv.DictReader` with `restval=""` so missing trailing columns don't crash
- Mapping config loaded once at startup, not per-row, to avoid YAML parse overhead
- Exit code 2 reserved for "config file invalid" to distinguish from data errors (1)

## How to Run / Verify

<Concrete commands or steps a reviewer can execute.>

Example:
```bash
pip install -e .
python -m csvconvert --in tests/fixtures/sample_in.csv \
                    --out /tmp/out.csv \
                    --map tests/fixtures/sample_map.yml
diff /tmp/out.csv tests/fixtures/expected_out.csv
```
