# Claude-Research-Space-Plugin

Claude Code plugin: research workflow — source logging, source summarization, and deep-dive primitives, with ten variants for different research modes — including an Obsidian-vault variant that doubles as a working vault. Public workspaces are auto-registered into the [Open-Research-Workspaces-Index](https://github.com/danielrosehill/Open-Research-Workspaces-Index).

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

### Tech research team (subagents)

Under `agents/tech-research-team/` — a 30-agent network for hardware/software stack evaluations, folded in from the former `Claude-Tech-Research-Team` repo. All agents are namespaced `tr-*` to avoid collisions.

- **Managers**: research-lead, prompt-organiser, output-manager, context-organiser, gitignore-manager
- **Input**: prompt-to-spec, transcript-cleaner, context-saver
- **Option exploration**: saas-finder, open-source-finder
- **Source finders (hardware)**: us-sources-finder, israel-sources-finder, amazon-uk, amazon-to-israel, aliexpress
- **Research**: evaluator, reputation-checker, oem-investigator, pn-checker, rrp-retrieval, support-check, durability-probe, deep-spec-probe
- **Compatibility**: mobile, linux-software-check, linux-hardware-check
- **Comparison & reporting**: comparisons, ranker, author
- **Specialists**: deployment-process

Pair these with the `stack`, `technical`, or `purchasing` workspace variants.

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
| `obsidian-vault` | The same research loop, scaffolded as a working [Obsidian](https://obsidian.md) vault — committed `.obsidian/` config, frontmatter schema, wikilink discipline, templates, canvas |

### The `obsidian-vault` variant

Optimised for two readers rather than one. It is the standard research scaffold — context, questions, sources, findings, outputs — expressed so that opening the folder in Obsidian gives a configured vault rather than a pile of markdown: graph colour groups per folder, backlinks that resolve, a properties schema the Properties panel understands, note templates wired to the core Templates plugin, and a canvas of the research loop.

No community plugins are required: each folder carries a hand-maintained map of content, so the vault works on a fresh install. `Meta/Dataview Queries.md` mirrors those views as live queries for anyone who has Dataview.

The scaffold's `CLAUDE.md` teaches the agent the vault conventions it would not otherwise follow — quoted link values in frontmatter, one claim per note, never linking a note that does not exist, and rewriting backlinks by hand when renaming from the shell (Obsidian only does that for renames it performs itself).

Also published standalone as [Obsidian-Research-Vault-Template](https://github.com/danielrosehill/Obsidian-Research-Vault-Template), a GitHub template repo, for use without the plugin. `template/obsidian-vault/` here is the source of truth; the standalone repo mirrors it.

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
