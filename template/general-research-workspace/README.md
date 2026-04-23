# Research Workspace (general-research-workspace variant)

Provisioned by the [research-space Claude Code plugin](https://github.com/danielrosehill/Claude-Research-Space-Plugin).

An openly-logged research question space. Q&A pattern: you ask questions, Claude gathers material and writes longform responses, paired records accumulate, and selected pairs are periodically consolidated into PDFs or reports.

## Layout

- `CLAUDE.md` — workspace instructions (variant: `general-research-workspace`)
- `questions/` — one markdown file per question, slug-keyed
- `answers/` — one markdown file per answer, same slug as the question
- `context/` — long-lived framing, definitions, prior-art notes
- `sources/` — logged sources referenced across answers
- `outputs/` — consolidated exports, PDFs, reports
- `notes/` — scratch thinking

## Primitives (installed globally by the plugin)

- `/research-space:research-init` — load state at session start
- `/research-space:source-log <ref>` — log a new source
- `/research-space:summarize-sources` — per-source summaries + synthesis rollup
- `/research-space:deep-dive <topic>` — structured investigation
- `/research-space:research-status` — workspace state report
- `/research-space:export-report` — compose a shareable or consolidated report
