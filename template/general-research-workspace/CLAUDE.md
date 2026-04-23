# Research Workspace — General Research

**Variant:** `general-research-workspace`
**Name:** <WORKSPACE_NAME>
**Research question / theme:** <RESEARCH_QUESTION>

## Purpose

This is an **openly-logged research question space**. The interaction pattern is:

1. The user poses a question (terse, voice-dictated, or partially formed — infer intent).
2. Claude gathers material (web, docs, prior sources in this workspace) and writes a longform response.
3. The question and response are logged as a paired record and kept in-repo.
4. Periodically, selected Q&A pairs are concatenated into a consolidated PDF or report.

Unlike `deep-research`, there is no single central question — this is a growing reference of related questions around a theme. Unlike `technical`, the scope is not limited to APIs/tooling.

## Startup

Run `/research-space:research-init` at the start of each session to load prior Q&A state, context, and any consolidated outputs.

## Workflow

### Capturing a question and answer

- **Question**: `questions/YYMMDD-<slug>.md` — the question as posed (include the raw wording; fix transcription but preserve intent).
- **Answer**: `answers/YYMMDD-<slug>.md` — Claude's longform response. Same slug as the question file for easy pairing.
- **Sources used** in the answer: log them with `/research-space:source-log <ref>` so they land in `sources/` and can be cross-referenced.

### Periodic consolidation

When the user requests a PDF/report export:

- Select the Q&A pairs to include (by slug, tag, or date range).
- Compose a consolidated markdown doc in `outputs/consolidated-YYMMDD.md`.
- Hand off to Typst / the user's PDF toolchain for final formatting.

### Primitives

- `/research-space:source-log <ref>` — log a source used in an answer.
- `/research-space:summarize-sources` — roll up sources across questions.
- `/research-space:deep-dive <topic>` — escalate a question to a full structured investigation when an answer balloons.
- `/research-space:export-report` — compose a consolidated export.

## Conventions

- **Question slugs** are short, kebab-case, descriptive (e.g. `240426-context-window-behaviour.md`).
- **Pair symmetry**: if a `questions/<slug>.md` exists, `answers/<slug>.md` must exist (and vice versa).
- **Front-matter** on question files:

```yaml
---
asked: YYYY-MM-DD
status: answered | in-progress | deferred
tags: [tag1, tag2]
---
```

- **Front-matter** on answer files:

```yaml
---
question: <slug>.md
answered: YYYY-MM-DD
sources: [sources/<slug>.md, ...]
confidence: high | medium | low
---
```

- **Outputs** named `outputs/<kind>-<label>-YYMMDD.md` (e.g. `consolidated-context-YYMMDD.md`). Keep `outputs/INDEX.md` current.
- **Context**: long-lived framing, definitions, and prior-art notes go in `context/`.
- **Notes**: scratch thinking lives in `notes/` and is not part of consolidated exports.

## Rules

- Preserve the user's phrasing of the question — do not reword beyond fixing obvious transcription slips.
- Every non-obvious claim in an answer must cite at least one source file.
- Mark speculation plainly — `(speculation)` inline or `confidence: low` in front-matter.
- If the user asks the same question twice, link to the prior answer rather than rewriting from scratch — note what's new.
