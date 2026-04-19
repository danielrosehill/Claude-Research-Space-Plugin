---
description: Log a new source (URL, document, quote, transcript, OSINT artifact) into the current research workspace's sources/ directory with consistent front-matter.
---

# /research-space:source-log

Append a new source entry to the research workspace rooted at the current working directory.

## Behavior

1. Verify a `sources/` directory exists (create if missing). If no `CLAUDE.md` is present in cwd, warn the user — they may not be inside a research workspace.
2. Parse `$ARGUMENTS` as free-form input. Extract or prompt for:
   - **URL or identifier** (required)
   - **Title**
   - **Type** — `web`, `document`, `paper`, `transcript`, `osint-artifact`, `dataset`, `quote`
   - **Relevance** — one-line why it matters
   - **Tags** — comma-separated
3. Write a markdown file at `sources/YYMMDD-<slug>.md` with YAML front-matter:
   ```yaml
   ---
   url: ...
   title: ...
   type: ...
   captured: YYYY-MM-DD
   tags: [...]
   ---
   ```
   Body contains the relevance note plus an empty `## Notes` section for later annotation.
4. If the workspace has an index file (`sources/INDEX.md`), append a row; otherwise create it with a header.
5. Print the relative path of the new source file.

## Notes

- Never fetch URL contents here — just log the reference. Use `/research-space:summarize-sources` for that.
- Slug: kebab-case of the title, max 50 chars.
- Preserve existing user-added fields in front-matter on re-runs.
