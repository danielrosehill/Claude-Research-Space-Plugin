---
description: Report the state of the current research workspace — source counts, recent outputs, variant, and anything flagged as stale or incomplete.
---

# /research-space:research-status

Produce a concise status report for the research workspace in cwd.

## Behavior

1. Verify cwd is a research workspace. Abort otherwise.
2. Gather:
   - Variant (from `CLAUDE.md`)
   - Source count, broken down by `type:` front-matter field
   - Sources with no `## Summary` section yet (candidates for `/research-space:summarize-sources`)
   - Recent outputs (last 5, newest first)
   - Recent log entries (last 3)
3. Print the report to stdout. Do not write files.
