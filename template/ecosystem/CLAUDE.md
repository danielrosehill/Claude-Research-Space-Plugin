# Research Workspace — Ecosystem Mapping

**Variant:** `ecosystem`
**Name:** <WORKSPACE_NAME>
**Ecosystem:** <ECOSYSTEM>

## Purpose

Map an ecosystem — the actors (companies, orgs, projects, people), their relationships, funding flows, and market segments. Output is typically an actor graph plus a narrative overview.

## Startup

Run `/research-space:research-init` at the start of each session.

## Workflow

Same primitives: `/research-space:source-log`, `/research-space:summarize-sources`, `/research-space:deep-dive`.

## Ecosystem-specific conventions

- Tag sources with `actor:<name>` and `segment:<segment>`.
- Maintain `context/actors.md` — canonical list of actors with one-line descriptors.
- Maintain `context/segments.md` — the segmentation taxonomy you're using.
- Suggested subdirs: `actors/<slug>/` for per-actor dossiers, `graph/` for relationship data (nodes.csv, edges.csv), `maps/` for rendered visualisations.
- Deep-dives can target a segment, a funding round, or a specific actor.

## Rules

- Every relationship edge in `graph/edges.csv` cites at least one source.
- Note each actor's founding date, HQ location, and size bracket when first logged.
- Distinguish confirmed relationships from inferred ones in the edge list (`confidence` column).
