---
type: context
title: Conventions
created: <DATE>
updated: <DATE>
tags:
  - meta
---

# Conventions

The contract between the two halves of this vault. Obsidian's graph, backlinks, properties panel, search filters and Dataview all read the frontmatter and the wikilinks below — so does the agent. Change something here and change it in both `CLAUDE.md` and `Meta/Dataview Queries.md`.

## Frontmatter

### Every note

```yaml
---
type: source            # source | question | finding | entity | output | context | note
title: Human readable title
created: 2026-08-12     # ISO date, never relative
updated: 2026-08-12     # bump on every substantive edit
tags:
  - research/<topic>
---
```

`type` is the single most load-bearing field: every view in the vault filters on it first.

### `type: source`

```yaml
url: https://example.com/filing
author: Jane Roe
published: 2026-03-01     # date the source was published, if known
retrieved: 2026-08-12     # date you actually fetched it
source-type: primary      # primary | secondary | dataset | interview | forum | official
reliability: high         # high | medium | low | unverified
paywalled: false
```

`retrieved` is not decoration. A source fetched a year ago and a source fetched today make different claims about the world; record which you have.

### `type: question`

```yaml
status: open              # open | in-progress | answered | abandoned
answered-by: []           # ["[[Finding note]]", ...] once settled
priority: normal          # high | normal | low
```

### `type: finding`

```yaml
confidence: high          # high | medium | low | speculative
sources: ["[[240812-acme-filing]]"]
answers: ["[[Does the exemption survive 2027?]]"]
```

A finding with an empty `sources` list is an assertion, not a finding. Either cite it or move it to `Notes/`.

### `type: entity`

```yaml
entity-type: org          # person | org | product | place | event
aliases: [Acme, Acme Corporation]
```

`aliases` feeds Obsidian's linker, so `[[Acme]]` resolves to the same note as `[[Acme Corp]]`.

### `type: output`

```yaml
format: pdf               # pdf | audio | markdown | slides
state: draft              # draft | review | final
audience: internal        # internal | client | public
```

## Link values in properties

Obsidian only renders a link inside frontmatter if it is **quoted**:

```yaml
sources: ["[[240812-acme-filing]]"]      # ✅ renders as a link
sources: [[[240812-acme-filing]]]        # ❌ invalid YAML
sources: [[240812-acme-filing]]          # ❌ parses as a nested list, no link
```

## Two placeholder syntaxes, and why `Templates/` is excluded

Notes in `Templates/` carry `{{date}}` — Obsidian's own Templates-plugin token, filled when a note is created from the template. It is deliberately unquoted, so the note you end up with has a real date that Dataview and Bases treat as a date rather than a string.

The cost is that the template file *itself* has frontmatter that is not valid YAML (`{{date}}` parses as a mapping key). `.obsidian/app.json` therefore sets `userIgnoreFilters: ["Templates/"]`, which keeps the folder out of search, the graph, backlinks and the quick switcher — so half-formed template frontmatter never pollutes a query and template boilerplate never turns up in a research search. Insert templates through the Templates plugin's own command, not the quick switcher.

Anything substituting into this vault must leave `{{date}}` alone and only fill the `<ANGLE_BRACKETED>` placeholders.

## Filenames

| Folder | Pattern | Example |
|--------|---------|---------|
| `Sources/` | `YYMMDD-<slug>.md` | `240812-acme-q2-filing.md` |
| `Outputs/` | `<kind>-<slug>-YYMMDD.md` | `report-tariff-exposure-240812.md` |
| Everything else | Readable title | `Tariff exemption expires 2027.md` |

Sources are date-prefixed because their identity is *when you captured this particular artefact*. Findings and entities are titled because their identity is what they say — and because the title is the link text every other note will use.

## Tags

Tags carry **state and topic**. Things get links, not tags: `[[Acme Corp]]`, never `#acme-corp`.

```
#research/<topic>      topical spine — the one tag every note carries
#status/verified       claim independently confirmed
#status/needs-check    flagged for verification
#status/contested      sources disagree — say so in the note body
#source/paywalled      capture the text; the URL may not be reachable later
#output/pdf            destined for a rendered deliverable
#output/audio          destined for narration
```

Nested tags (`research/tariffs`) collapse in Obsidian's tag pane and filter with a prefix search — flat tag soup does neither. Add new tags to `Meta/Tags.md` when you coin them.

## Linking

- Wikilink on **first mention** of any entity, source or finding in a note.
- Never link a note that does not exist. Create the stub or drop the link — unresolved links clutter the graph and hide real gaps.
- Use aliases for readability: `[[240812-acme-q2-filing|Acme's Q2 filing]]`.
- Embed sparingly: `![[Finding note]]` pulls the whole note into the reading view. Good for assembling an output, bad everywhere else.

## Maps of content

Each folder has an index note (`Questions/Questions.md`, `Sources/Sources.md`, …) listing what it contains. These are maintained **by hand** so the vault works with zero community plugins on a fresh install.

`Meta/Dataview Queries.md` mirrors the same views as live queries for anyone who has Dataview. Both, or the vault degrades for one audience or the other.

## Renaming

Obsidian rewrites backlinks when it does the rename. Nothing else does. Renaming from the shell or from an agent means finding and rewriting every reference yourself:

```bash
rg -l "\[\[Old Note Name" .
```

Catch aliases (`[[Old|display]]`) and embeds (`![[Old]]`) too, and land the rename and the rewrite in one commit.
