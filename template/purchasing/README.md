# Research Workspace (purchasing variant)

Provisioned by the [research-space Claude Code plugin](https://github.com/danielrosehill/Claude-Research-Space-Plugin).

Structured research to support a purchasing decision. Flow: **spec → candidates → shortlist → decision**, all logged.

## Layout

- `CLAUDE.md` — workspace instructions (variant: `purchasing`)
- `context/` — `spec.md`, `criteria.md`, `constraints.md`
- `candidates/<slug>/` — per-candidate dossiers
- `shortlist/` — scoring matrix and tradeoff notes
- `decisions/` — final decision records
- `sources/` — logged sources (product pages, reviews, teardowns)
- `outputs/` — purchase briefs, exports
- `notes/` — scratch thinking

## Primitives (installed globally by the plugin)

- `/research-space:research-init`
- `/research-space:source-log <ref>`
- `/research-space:summarize-sources`
- `/research-space:deep-dive <topic>`
- `/research-space:research-status`
- `/research-space:export-report`
