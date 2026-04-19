---
description: Initialize a session in the current research workspace — load CLAUDE.md, context, sources index, and prior outputs so the agent has full state.
---

# /research-space:research-init

Run at the start of every session in a research workspace to bootstrap agent context.

## Behavior

1. Verify cwd is a research workspace (has `CLAUDE.md`). If not, tell the user and stop.
2. Read:
   - `CLAUDE.md` (workspace instructions + variant)
   - All files under `context/`
   - `sources/INDEX.md` if present, else list `sources/*.md`
   - `outputs/INDEX.md` if present, else list `outputs/*.md`
   - Most recent file under `logs/` if present
3. Print a compact status briefing:
   - Variant
   - Source count by type
   - Recent outputs (last 3)
   - Open questions / gaps flagged in the most recent deep-dive (if any)
4. Do not modify files. This is read-only orientation.
