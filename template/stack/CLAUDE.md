# Research Workspace — Stack Research

**Variant:** `stack`
**Name:** <WORKSPACE_NAME>
**Technology area:** <STACK_AREA>

## Purpose

Evaluate and track a technology stack or a specific tooling area: candidate tools, their maturity, fit criteria, tradeoffs, and an eventual recommendation or shortlist.

## Startup

Run `/research-space:research-init` at the start of each session.

## Workflow

Same primitives: `/research-space:source-log`, `/research-space:summarize-sources`, `/research-space:deep-dive`.

## Stack-specific conventions

- Tag each source with the tool/product name: `tool:<name>`.
- Maintain `context/criteria.md` — the dimensions you're evaluating on (cost, licensing, integration, perf, community, etc.).
- Suggested subdirs: `candidates/<tool>/` for per-tool dossiers, `comparisons/` for matrix outputs.
- A deep-dive on the stack as a whole should produce a **scoring matrix** plus a recommendation.

## Rules

- Separate vendor-authored claims from independent benchmarks in summaries.
- Record licence, pricing tier, and last-release-date at time of capture — these change fast.
- Reject sources older than 24 months unless they document fundamentals.
