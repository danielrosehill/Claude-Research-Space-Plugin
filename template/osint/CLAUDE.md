# Research Workspace — OSINT Investigation

**Variant:** `osint`
**Name:** <WORKSPACE_NAME>
**Subject:** <INVESTIGATION_SUBJECT>
**Geographic scope:** <SCOPE>

## Purpose

Open-source intelligence workspace. Identify, verify, and cross-reference publicly available information about the subject. Chain-of-custody and provenance matter more than narrative polish.

## Startup

Run `/research-space:research-init` at the start of each session.

## Workflow

Same primitives: `/research-space:source-log`, `/research-space:summarize-sources`, `/research-space:deep-dive`.

## OSINT-specific conventions

- Every source gets a provenance note: **captured-from**, **archive-url** (Wayback/archive.today), **capture-date**, **original-language**.
- Evidence hierarchy: primary (documents, images, video, sensor data) > first-hand testimony > secondary reporting > aggregators. Mark clearly in front-matter `type`.
- Suggested subdirs (create as needed): `evidence/` for downloaded artifacts, `imagery/`, `timeline/`, `actors/`, `locations/`.
- Maintain `sources/INDEX.md` grouped by evidence tier.
- Red-team your own conclusions: each deep-dive must include a "what would disprove this" section.

## Rules

- Never assert identity, location, or intent without at least two independent primary sources.
- Mark every claim with confidence: `confirmed`, `probable`, `possible`, `unverified`.
- Archive every web source immediately — link rot is the enemy.
- No doxing private individuals. Public officials and corporate entities only.
