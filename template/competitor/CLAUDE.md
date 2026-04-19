# Research Workspace — Competitor Research

**Variant:** `competitor`
**Name:** <WORKSPACE_NAME>
**Competitor:** <COMPETITOR>
**Framing company:** <OUR_COMPANY>

## Purpose

Structured intelligence on a specific competitor (or a small set). Product, positioning, pricing, team, funding, go-to-market, strengths/weaknesses, and strategic inference.

## Startup

Run `/research-space:research-init` at the start of each session.

## Workflow

Same primitives: `/research-space:source-log`, `/research-space:summarize-sources`, `/research-space:deep-dive`.

## Competitor-specific conventions

- Tag sources with `lens:<product|pricing|team|funding|gtm|tech|comms>`.
- Maintain `context/lenses.md` — which analytical lenses you care about and why.
- Suggested subdirs: `analysis/` for per-lens writeups, `inputs/` for raw captured artifacts, `timeline/` for their product/comms history.
- A deep-dive should end in an "implications for us" section framed around the framing company.

## Rules

- Never rely on a single source for a competitive claim — cross-reference.
- Separate **facts** (what they do/say) from **inference** (what we think it means) in every output.
- Stay ethical: public sources only. No social-engineering, no pretext outreach, no scraping gated material.
