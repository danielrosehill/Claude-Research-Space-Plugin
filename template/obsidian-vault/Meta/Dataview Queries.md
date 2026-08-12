---
type: context
title: Dataview Queries
created: <DATE>
updated: <DATE>
tags:
  - meta
---

# Dataview Queries

Optional. The vault does not need [Dataview](https://github.com/blacksmithgu/obsidian-dataview) — the maps of content in each folder cover the same ground statically. If you have the plugin installed, these render live and you can stop maintaining those by hand.

If you do **not** have Dataview, the blocks below show as plain code. Nothing breaks.

## Open questions, highest priority first

```dataview
TABLE priority, status, updated
FROM "Questions"
WHERE type = "question" AND status != "answered" AND status != "abandoned"
SORT priority ASC, updated DESC
```

## Findings that need checking

```dataview
TABLE confidence, sources, updated
FROM "Findings"
WHERE type = "finding" AND (confidence = "low" OR confidence = "speculative" OR contains(tags, "status/needs-check"))
SORT updated DESC
```

## Findings with no source — fix or demote these

```dataview
LIST
FROM "Findings"
WHERE type = "finding" AND (!sources OR length(sources) = 0)
```

## Sources by reliability

```dataview
TABLE source-type AS "Kind", reliability, retrieved, paywalled
FROM "Sources"
WHERE type = "source"
SORT reliability ASC, retrieved DESC
```

## Sources retrieved more than 6 months ago

Anything time-sensitive here may have moved on.

```dataview
TABLE retrieved, url
FROM "Sources"
WHERE type = "source" AND retrieved < date(today) - dur(6 months)
SORT retrieved ASC
```

## Entities and what links to them

```dataview
TABLE entity-type AS "Kind", length(file.inlinks) AS "Backlinks"
FROM "Entities"
WHERE type = "entity"
SORT length(file.inlinks) DESC
```

## Outputs in flight

```dataview
TABLE format, state, audience, updated
FROM "Outputs"
WHERE type = "output" AND state != "final"
SORT updated DESC
```

## Orphans — notes nothing links to

Not automatically a problem, but usually a note that never got wired in.

```dataview
LIST
FROM "" 
WHERE length(file.inlinks) = 0 AND file.name != "Home" AND !contains(file.folder, "Templates") AND !contains(file.folder, "Meta")
```

## Everything touched this week

```dataview
TABLE type, updated
FROM ""
WHERE updated >= date(today) - dur(7 days)
SORT updated DESC
```

---

**Bases.** Obsidian's built-in Bases plugin covers much of the same ground without a community plugin, driven by the same frontmatter properties. No `.base` file ships here — the format is worth building against your own Obsidian version rather than inheriting one that may not match. The `type` / `status` / `confidence` / `reliability` properties are already shaped for it.
