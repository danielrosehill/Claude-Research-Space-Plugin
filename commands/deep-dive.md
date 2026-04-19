---
description: Run a deep-dive investigation on a specific topic, entity, or question inside the current research workspace. Produces a structured report in outputs/.
---

# /research-space:deep-dive

Drive a focused investigation on a single topic, question, or entity using the workspace's accumulated sources plus targeted new research.

## Behavior

1. Verify cwd is a research workspace. If not, abort.
2. Parse `$ARGUMENTS` as the deep-dive subject (free-form). If empty, prompt the user.
3. Load context: read `CLAUDE.md`, `context/**/*.md`, and any `sources/INDEX.md` to orient on what's already known.
4. Plan the investigation:
   - Identify sub-questions that break the subject into tractable pieces.
   - Note which existing sources already address each sub-question.
   - Identify gaps that require new sources (web research, document retrieval, OSINT lookups — whichever the variant supports).
5. Execute the plan:
   - For each gap, gather new sources and log them via the same conventions as `/research-space:source-log`.
   - Summarize new material inline.
6. Write a structured report at `outputs/deep-dive-<slug>-YYMMDD.md` containing:
   - **Subject** — one paragraph framing
   - **Key findings** — bullet list, each linked to a source
   - **Sub-question analysis** — one short section per sub-question
   - **Confidence & gaps** — what's solid, what's shaky, what's still unknown
   - **Recommended next steps**
7. Append the report path to `outputs/INDEX.md`.

## Notes

- Stay tight — this is not a book report. Target 800–2000 words.
- Link every claim to at least one source in `sources/` (relative path).
- Variant awareness: OSINT deep-dives weight primary-source provenance; competitor dives weight strategic inference; ecosystem dives weight actor-graph completeness.
