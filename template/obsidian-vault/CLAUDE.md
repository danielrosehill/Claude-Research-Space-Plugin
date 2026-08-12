# Research Workspace — Obsidian Vault

**Variant:** `obsidian-vault`
**Name:** <WORKSPACE_NAME>
**Central question:** <RESEARCH_QUESTION>

## Purpose

A research workspace that is simultaneously two things:

1. **An agent scaffold** — the same research loop as the other `research-space` variants: capture sources, answer questions, synthesise findings, produce outputs (usually PDF, sometimes audio).
2. **An Obsidian vault** — the human opens this folder in Obsidian and gets a working, pre-configured vault: graph view, backlinks, properties, templates, a canvas.

Neither half is decoration. Write for both readers on every edit: an agent that greps the filesystem, and a person navigating by link, tag, and graph.

## Startup

Run `/research-space:research-init` at the start of a session to load `Context/`, the source index, and prior outputs.

If `Home.md` is stale (new notes not listed in the maps of content), refresh it — see *Maintaining the indexes* below.

## Vault layout

| Folder | Holds | Note type |
|--------|-------|-----------|
| `Context/` | Long-lived framing: scope, glossary, background. Rarely changes. | `context` |
| `Questions/` | One note per open question. The spine of the research. | `question` |
| `Sources/` | One note per source. Never more than one source per note. | `source` |
| `Findings/` | Atomic claims, each citing sources. The reusable unit. | `finding` |
| `Entities/` | People, organisations, products, places. Includes contacts. | `entity` |
| `Outputs/` | Deliverables — reports, drafts headed for PDF or audio. | `output` |
| `Notes/` | Scratch. Not part of any deliverable. | `note` |
| `Assets/` | Attachments — images, PDFs, audio. Obsidian writes pastes here. | — |
| `Templates/` | Note shapes for Obsidian's Templates plugin **and** for you. | — |
| `Meta/` | Conventions, tag taxonomy, optional Dataview queries. | — |

`Home.md` is the vault entry point. `Research Map.canvas` is the visual overview.

## The one rule that makes this an Obsidian vault

**Every note gets YAML frontmatter, and notes link to each other with `[[wikilinks]]`.**

Obsidian's graph, backlinks panel, properties UI, search filters and Dataview all read exactly these two things. A note with no frontmatter and no links is invisible to every Obsidian affordance — it is a text file sitting in a vault, not part of it.

Full schema: `Meta/Conventions.md`. Read it before writing your first note in a session.

Minimum on every note:

```yaml
---
type: source            # source | question | finding | entity | output | context | note
title: Human readable title
created: 2026-08-12
updated: 2026-08-12
tags:
  - research/<topic>
---
```

Link values in frontmatter must be quoted to render as links: `sources: ["[[240812-acme-filing]]"]`.

## Workflow

1. **Frame** — write or update `Context/Scope.md`. Add open questions as notes in `Questions/`.
2. **Capture** — `/research-space:source-log <url-or-ref>` writes a `Sources/` note. Fill `reliability` and `source-type` honestly; those fields are the reason the folder is worth having.
3. **Extract** — pull each substantive claim out of the source into its own `Findings/` note, with `sources:` pointing back. One claim per note. This is what makes findings reusable across outputs.
4. **Answer** — when a question is settled, set its `status: answered` and link the findings that settle it.
5. **Synthesise** — `/research-space:summarize-sources`, `/research-space:deep-dive <topic>`.
6. **Deliver** — `/research-space:export-report`, then render to PDF or audio (see *Outputs* below).

## Writing notes

- **One idea per note.** A 3,000-word note is a document, not a vault note. Split it and link the parts.
- **Filenames are the link text.** Sources use `YYMMDD-<slug>.md`; everything else uses a readable title, e.g. `Findings/Tariff exemption expires 2027.md`. Do not rename casually — see below.
- **Link on first mention.** The first time a note mentions an entity, a source, or another finding, wikilink it. Backlinks are how the human navigates.
- **Prefer links over tags for things; tags for status and topic.** `[[Acme Corp]]` is a thing. `#status/verified` is a state.
- **Never fabricate a link target.** If `[[Some Entity]]` does not exist yet, either create the stub note or do not link it. Unresolved links pollute the graph.
- **Never write an example wikilink in prose or in an HTML comment.** Obsidian indexes links inside `<!-- -->` exactly as it indexes them anywhere else, so an illustrative `[[Note title]]` becomes a real unresolved node in the graph. Put examples in backticks or write them out in words.

## Renaming and moving

Obsidian rewrites backlinks automatically when *it* renames a file. You are not Obsidian. If you rename or move a note from the shell:

```bash
rg -l "\[\[Old Note Name" .        # find every referring note
```

then update each reference, including aliases (`[[Old Note Name|display text]]`) and embeds (`![[Old Note Name]]`). Do the rename and the link rewrite in the same commit.

## Maintaining the indexes

Each content folder has a map-of-content note (`Questions/Questions.md`, `Sources/Sources.md`, …) listing its notes. These are maintained **statically**, by you, so the vault works for users who have no community plugins installed.

`Meta/Dataview Queries.md` holds the same views as live Dataview queries, for users who do have it. When you add a note, add its line to the relevant MOC. When you add a *kind* of view, add it to both places.

## Outputs

`Outputs/` notes are drafts destined to leave the vault:

- **PDF** — Typst is the house renderer. Keep the markdown clean: no Obsidian-only syntax (`[[wikilinks]]`, `![[embeds]]`, callouts) in a note headed for PDF, or resolve it during export.
- **Audio** — narration scripts get `format: audio` and should read aloud cleanly: no tables, no bare URLs, expand abbreviations.

Set `state: draft` until the deliverable ships, then `state: final`.

## Rules

- Every non-obvious claim in an output cites at least one `Sources/` note.
- Distinguish primary sources from secondary commentary via `source-type` in frontmatter.
- Flag uncertainty plainly: `confidence: speculative` in frontmatter, `(speculation)` inline.
- Record the date you verified something, not just the date you wrote it. `retrieved` on sources; `updated` on everything.

## Hands off

- **`.obsidian/workspace.json`** — per-machine UI layout, gitignored. Never create or commit it.
- **`.obsidian/*.json` generally** — committed on purpose so a clone opens configured. Change these deliberately and say why in the commit, never as a side effect.
- **`.trash/`** — Obsidian's local trash. Gitignored. Do not resurrect from it silently.
