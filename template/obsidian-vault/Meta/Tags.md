---
type: context
title: Tags
created: <DATE>
updated: <DATE>
tags:
  - meta
---

# Tags

The controlled vocabulary. Coining a tag means adding it here — otherwise the tag pane fills with near-synonyms and filtering stops working.

Things get **links** (`[[Acme Corp]]`), not tags. Tags are for state and topic only.

## Topic

| Tag | Meaning |
|-----|---------|
| `#research/<topic>` | The topical spine. Every note carries exactly one. Add sub-topics as they earn their keep. |

Replace `<topic>` with this workspace's areas and list them here as you go:

- `#research/` — 

## Status

| Tag | Meaning |
|-----|---------|
| `#status/verified` | Independently confirmed against a second source |
| `#status/needs-check` | Flagged for verification; do not ship in an output as-is |
| `#status/contested` | Sources disagree. The note body must say who says what |
| `#status/stale` | Was true; the underlying situation has moved |

## Source handling

| Tag | Meaning |
|-----|---------|
| `#source/paywalled` | Capture the relevant text in the note — the URL may not be reachable later |
| `#source/archived` | An archive.org or local copy exists; link it in the note |
| `#source/machine-translated` | Read in translation. Treat wording as approximate |

## Output routing

| Tag | Meaning |
|-----|---------|
| `#output/pdf` | Destined for a rendered PDF. Keep the markdown portable — no wikilinks, embeds or callouts |
| `#output/audio` | Destined for narration. No tables, no bare URLs, abbreviations expanded |

## Housekeeping

| Tag | Meaning |
|-----|---------|
| `#meta` | Notes about the vault itself, not the research |
