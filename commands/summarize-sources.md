---
description: Summarize logged sources in the current research workspace. Produces per-source summaries and a rolled-up synthesis in outputs/.
---

# /research-space:summarize-sources

Walk `sources/` in the current research workspace and produce summaries.

## Behavior

1. Verify cwd is a research workspace (has `CLAUDE.md` + `sources/`). Abort with a clear message if not.
2. Parse `$ARGUMENTS` for optional filters: `--type=<type>`, `--tag=<tag>`, `--since=<YYMMDD>`, `--only=<slug>`.
3. For each matching source file:
   - If it has no `## Summary` section yet, fetch/read the referenced content (URL via WebFetch, local files via Read) and write a concise 5–10 line summary.
   - If it already has one, skip unless `--refresh` is passed.
4. After per-source summaries are written, produce a rollup at `outputs/synthesis-YYMMDD.md` that:
   - Lists the sources covered in this pass
   - Pulls out recurring themes, contradictions, and open questions
   - Links back to each source file by relative path
5. Append a line to `logs/summarize-YYMMDD.log` (create `logs/` if needed) recording the run.

## Notes

- Keep per-source summaries factual and terse — 150 words max.
- Synthesis is for the user's thinking, not for publication. Mark uncertainties plainly.
- Respect the variant-specific lenses in the workspace's CLAUDE.md (OSINT workspaces care about provenance; stack/tech-radar workspaces care about maturity signals; ecosystem workspaces about actor relationships).
