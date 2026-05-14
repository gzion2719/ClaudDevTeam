---
schema_version: "1.0"
role: QA
date: YYYY-MM-DD
project: <project name or slug>
---

<!--
SKELETON — sample handoff file conforming to ADR-0001.

Replace each section's placeholder text with real content.
Do NOT delete section headers — every required section must remain present,
even if its body is "none" or "not applicable".

This file is read by the Architect role in handoff-intake (revision-cycle) mode.
-->

# QA_REPORT — <project name>

## Go / No-Go

<Go | No-Go> — <one sentence rationale>

Example:
> No-Go — one critical finding (silent data loss on UTF-16 inputs) must be
> resolved before this can ship; majors and minors can be deferred.

## Findings

### Critical (blocks revision cycle)

<List, or "none".>

Example:
- Source CSVs encoded as UTF-16 are silently truncated at the first null byte
  — `CsvReader` opens with default encoding. Repro: `tests/fixtures/utf16_in.csv`.

### Major (should fix)

<List, or "none".>

Example:
- No validation that mapping YAML references existing source columns —
  typo in mapping produces empty target column with no warning.
- Exit code 1 used for both "input file missing" and "row transform failed" —
  callers can't distinguish.

### Minor (nice to fix)

<List, or "none".>

Example:
- Log message "processed N rows" uses inconsistent number formatting
  (sometimes with thousand separators, sometimes without).

## Validation Performed

<What was actually checked. Be concrete — tests run, files reviewed, interfaces verified.>

Example:
- Ran the "How to Run" command from DEV_SUMMARY.md against `sample_in.csv` — passed
- Ran adversarial inputs: empty CSV, UTF-16 CSV, CSV with trailing whitespace columns
- Reviewed `RowMapper` source against the deviation noted in DEV_SUMMARY.md — implementation matches
- Did NOT verify: behavior under 2GB source file (resource-intensive — flagging as untested)

## Recommended Next Steps for Architect

<Specific and actionable. Not "review the design" — name the decision to make.>

Example:
- Decide: should the tool detect non-UTF-8 encodings and fail loudly, or attempt detection?
  This is an architecture-level decision because it affects the `CsvReader` interface.
- Amend ARCH_BRIEF.md "Non-Negotiables" with the encoding policy chosen above.
- Consider splitting exit codes 1 and 2 further: 1=I/O, 2=config, 3=data — needs your call.
