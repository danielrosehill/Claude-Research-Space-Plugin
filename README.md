# Claude-Research-Space-Plugin

Claude Code plugin: research workflow — source logging, source summarization, and deep-dive primitives, with seven variants for different research modes.

Part of the [danielrosehill Claude Code marketplace](https://github.com/danielrosehill/Claude-Code-Plugins).

## What you get

### Primitives (globally available once installed)

Under `/research-space:*`:

- `research-init` — load workspace state at session start
- `source-log` — log a new source (URL, document, quote, transcript, OSINT artifact) into `sources/`
- `summarize-sources` — per-source summaries + a rolled-up synthesis in `outputs/`
- `deep-dive` — structured investigation on a topic, produces a report
- `research-status` — compact state report for the current workspace
- `export-report` — compose a shareable report from accumulated outputs

### Provisioning skill

- `/research-space:new-workspace <name> [--variant=<variant>] [--local-only] [--private]`

Scaffolds a new research workspace (CLAUDE.md + context/sources/outputs/notes), personalises from `~/.claude/CLAUDE.md`, and by default creates a public GitHub repo.

## Variants

| Variant | Use case |
|---------|----------|
| `deep-research` | General-purpose investigation around a central question (default) |
| `technical` | APIs, libraries, protocols, hardware — version-sensitive technical research |
| `osint` | Open-source intelligence with provenance, chain-of-custody, red-teaming |
| `georeaction` | Comparative regional reactions to an event or policy |
| `stack` | Tool/stack evaluation — criteria, candidates, scoring |
| `ecosystem` | Actor mapping — companies, orgs, relationships, segments |
| `competitor` | Competitor intelligence — product, pricing, team, positioning |

## Pattern

Primitives live in the plugin and are reachable from any cwd. Workspace scaffolds are shipped as plain data (no `.claude/` tree). Plugin updates never touch your workspace data.

See [PLAN.md in Claude-Workspace-Reshaping-190426](https://github.com/danielrosehill/Claude-Workspace-Reshaping-190426) for the full pattern spec this plugin follows.

## Install

```
/plugin marketplace add danielrosehill/Claude-Code-Plugins
/plugin install research-space
```

## License

MIT.
