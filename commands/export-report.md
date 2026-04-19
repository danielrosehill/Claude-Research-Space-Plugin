---
description: Export the current research workspace's outputs into a single cohesive report (markdown, optionally rendered with Typst if the workspace is configured for it).
---

# /research-space:export-report

Compose a clean, shareable report from the workspace's outputs.

## Behavior

1. Verify cwd is a research workspace. Abort otherwise.
2. Parse `$ARGUMENTS` for `--format=md|typst|pdf` (default `md`) and `--title=<title>` (default: workspace name).
3. Read `outputs/INDEX.md` (or `outputs/*.md` if no index) and assemble sections in index order.
4. Write `outputs/report-YYMMDD.md` with:
   - Title + date
   - Executive summary (pulled from most recent synthesis/deep-dive)
   - Body — concatenated outputs with de-duplicated headers
   - Source appendix — list of all sources with URLs
5. If `--format=typst` or `--format=pdf` and `scripts/` or `typst/` exists in the workspace, shell out to the workspace's own Typst pipeline. Otherwise print a hint to install it.
6. Print the report path.

## Notes

- Do not include unresolved TODOs or gap placeholders in the final report — flag them separately in the printout for the user.
