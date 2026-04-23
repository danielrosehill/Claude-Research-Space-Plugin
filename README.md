# Claude-Research-Space-Plugin

Claude Code plugin: research workflow — source logging, source summarization, and deep-dive primitives, with nine variants for different research modes. Public workspaces are auto-registered into the [Open-Research-Workspaces-Index](https://github.com/danielrosehill/Open-Research-Workspaces-Index).

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

- `/research-space:new-workspace <name> [--variant=<variant>] [--local-only] [--private] [--no-index]`

Scaffolds a new research workspace (CLAUDE.md + variant-specific layout), personalises from `~/.claude/CLAUDE.md`, by default creates a **public** GitHub repo, and — for public workspaces — appends an entry to `Open-Research-Workspaces-Index`. Use `--private` for a private repo (skips index registration); `--no-index` to publish publicly but skip the index.

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
| `purchasing` | Structured purchase research — spec → candidates → shortlist → decision |
| `general-research-workspace` | Openly-logged Q&A research space — user asks, Claude writes longform answers, pairs periodically consolidated into PDFs |

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
