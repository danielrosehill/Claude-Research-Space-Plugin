# Research Workspace — Georeaction Research

**Variant:** `georeaction`
**Name:** <WORKSPACE_NAME>
**Event / policy:** <EVENT>
**Regions compared:** <REGIONS>

## Purpose

Comparative analysis of how different geographic regions (countries, blocs, cities) react to a given event, policy, announcement, or trend. Goal: understand the delta in framing, sentiment, and response across regions.

## Startup

Run `/research-space:research-init` at the start of each session.

## Workflow

Same primitives: `/research-space:source-log`, `/research-space:summarize-sources`, `/research-space:deep-dive`.

## Georeaction-specific conventions

- Tag every source with `region:<region>` and `language:<lang>` in front-matter.
- Summaries should surface **framing** (how the event is characterised), **sentiment**, **stated policy response**, and **actors quoted**.
- Synthesis passes should produce a **comparison matrix** (region × dimension) in outputs.
- Suggested subdirs: `regions/<region>/` for per-region briefings, `timeline/` for chronology.

## Rules

- Treat translated sources explicitly — record translation tool or translator, retain the original-language excerpt.
- Do not infer national sentiment from a single outlet. Minimum two sources per region per claim.
- Separate government reaction, media reaction, and public/social reaction in summaries.
