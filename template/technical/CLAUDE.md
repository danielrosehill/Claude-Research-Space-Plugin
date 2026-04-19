# Research Workspace — Technical Research

**Variant:** `technical`
**Name:** <WORKSPACE_NAME>
**Subject:** <TECHNICAL_SUBJECT>

## Purpose

Technical investigation: APIs, libraries, protocols, hardware, architecture patterns. Emphasis on authoritative docs, benchmarks, and reproducible claims.

## Startup

Run `/research-space:research-init` at the start of each session.

## Workflow

Same three primitives as all research-space workspaces: `/research-space:source-log`, `/research-space:summarize-sources`, `/research-space:deep-dive`.

## Technical-specific conventions

- Prefer primary docs (official docs, RFCs, source code, first-party blog posts) over aggregators.
- Record **version**, **platform**, and **date** in source front-matter when technical claims are version-sensitive.
- For benchmarks, note methodology and hardware in the source summary.
- `questions/` folder (optional) can collect open technical questions awaiting deep-dive.
- `ideas/` folder (optional) for design sketches and notes-to-self.

## Rules

- Never paraphrase API syntax without linking the exact doc page.
- Deprecation warnings: if a source is older than 18 months for a fast-moving tech, flag it in the summary.
