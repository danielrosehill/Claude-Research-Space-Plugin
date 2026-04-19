# Research Workspace (deep-research variant)

Provisioned by the [research-space Claude Code plugin](https://github.com/danielrosehill/Claude-Research-Space-Plugin).

## Layout

- `CLAUDE.md` — workspace instructions (variant: `deep-research`)
- `context/` — long-lived framing (scope, criteria, taxonomies)
- `sources/` — logged sources, one markdown file per source
- `outputs/` — synthesis passes, deep-dive reports, exports
- `notes/` — scratch thinking

## Primitives (installed globally by the plugin)

- `/research-space:research-init` — load state at session start
- `/research-space:source-log <ref>` — log a new source
- `/research-space:summarize-sources` — per-source summaries + synthesis rollup
- `/research-space:deep-dive <topic>` — structured investigation
- `/research-space:research-status` — workspace state report
- `/research-space:export-report` — compose a shareable report
