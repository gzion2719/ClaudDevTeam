---
schema_version: "1.0"
role: Architect
date: YYYY-MM-DD
project: <project name or slug>
---

<!--
SKELETON — sample handoff file conforming to ADR-0001.

Replace each section's placeholder text with real content.
Do NOT delete section headers — every required section must remain present,
even if its body is "none" or "not applicable".

This file is read by the Developer role in handoff-intake mode.
-->

# ARCH_BRIEF — <project name>

## System Overview

<1-3 sentence description of what is being built and why.>

Example:
> A CLI tool that converts CSV exports from system A into the import format
> expected by system B, run nightly on a Windows scheduled task.

## Components

<List of named components, one line of responsibility each.>

Example:
- `CsvReader` — streams rows from the source CSV, handles UTF-8 BOM
- `RowMapper` — applies field transforms defined in `config/mapping.yml`
- `Writer` — emits the target CSV with the required header row
- `Cli` — argument parsing, exit codes, logging

## Interfaces

<Who calls what, with data shape. Be specific about types and edge cases.>

Example:
- `CsvReader.read() -> Iterator[Row]` where `Row = dict[str, str]`
- `RowMapper.transform(Row) -> Row | None` (None means "skip this row")
- `Writer.write(Iterator[Row]) -> None`, writes to path from CLI arg

## Technology Choices

<Language, frameworks, key libraries. Include rationale for non-obvious picks.>

Example:
- Python 3.11 — already used elsewhere in the org
- `csv` stdlib over `pandas` — no need for in-memory dataframe, streaming is enough
- `pyyaml` for the mapping config — same dependency as adjacent tools

## Non-Negotiables

<Hard constraints the Developer must NOT deviate from. Include rationale.>

Example:
- Must run on Windows (no Linux-only path separators or `os` calls)
- Must not load entire CSV into memory — source files reach 2GB
- All transforms must be expressible in YAML — no Python-coded mappers

## Open Questions / Assumptions

<Anything unresolved. Developer should flag if any of these blocks implementation.>

Example:
- Assumption: source CSV header row is always present (not verified with sample data)
- Open: how should the tool handle rows whose mapping fails — skip + log, or hard fail?
