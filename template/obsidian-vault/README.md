# <WORKSPACE_NAME>

An AI research workspace that is also an Obsidian vault.

**Central question:** <RESEARCH_QUESTION>

Provisioned by the [research-space Claude Code plugin](https://github.com/danielrosehill/Claude-Research-Space-Plugin) (variant: `obsidian-vault`).

## Two front doors

**Open it in Obsidian.** `Vault → Open folder as vault → <this folder>`. `.obsidian/` is committed, so the graph colours, templates, properties panel and attachment folder are already configured. Start at `Home.md` or open `Research Map.canvas`.

**Open it in Claude Code.** `cd <this folder> && claude`. `CLAUDE.md` teaches the agent the vault conventions — frontmatter schema, link discipline, where each kind of note goes — so what the agent writes is navigable in Obsidian rather than merely present on disk.

Neither is the "real" interface. The vault is the artefact; both are ways in.

## Layout

```
Home.md               entry point — maps of content
Research Map.canvas   visual overview

Context/              scope, glossary, background — long-lived framing
Questions/            one note per open question (the spine)
Sources/              one note per source, with reliability + provenance
Findings/             atomic claims, each citing sources
Entities/             people, organisations, products, places, contacts
Outputs/              deliverables headed for PDF or audio
Notes/                scratch

Assets/               attachments (Obsidian pastes land here)
Templates/            note shapes, for Obsidian's Templates plugin and the agent
Meta/                 conventions, tag taxonomy, optional Dataview queries
```

## Conventions

Every note carries YAML frontmatter and links to its neighbours with `[[wikilinks]]`. That is what feeds the graph, the backlinks panel, the properties view and search filters — it is the difference between a vault and a folder of markdown.

The schema, the tag taxonomy and the link rules live in [`Meta/Conventions.md`](Meta/Conventions.md). Read that before adding notes by hand.

## Plugins

No community plugins are required. The maps of content in each folder are maintained by hand (or by the agent), so the vault works on a fresh Obsidian install.

If you use **Dataview**, [`Meta/Dataview Queries.md`](Meta/Dataview%20Queries.md) has the same views as live queries — open questions, unverified findings, sources by reliability, orphan notes.

## Workflow

With the `research-space` plugin installed:

| Command | Does |
|---------|------|
| `/research-space:research-init` | Load workspace state at session start |
| `/research-space:source-log <url>` | Capture a source into `Sources/` |
| `/research-space:summarize-sources` | Per-source summaries + synthesis |
| `/research-space:deep-dive <topic>` | Structured sub-investigation |
| `/research-space:research-status` | Compact state report |
| `/research-space:export-report` | Compose a shareable report |

Without it, the vault still stands on its own — the conventions are documented, not encoded in tooling.

## Git

`.obsidian/workspace.json` and `.trash/` are gitignored: they are per-machine UI state and would conflict on every pull. Everything else in `.obsidian/` is committed on purpose.
