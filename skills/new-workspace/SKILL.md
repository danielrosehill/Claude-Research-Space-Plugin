---
name: new-workspace
description: Provision a new research-space workspace on disk. Use when the user wants to start a new research project (deep-research, technical, osint, georeaction, stack, ecosystem, or competitor). Accepts a workspace name and optional variant. Scaffolds the workspace, personalises CLAUDE.md from the user's global memory, and (by default) creates a GitHub repo.
disable-model-invocation: true
allowed-tools: Bash(mkdir *), Bash(cp *), Bash(cat *), Bash(git init *), Bash(git add *), Bash(git commit *), Bash(gh repo create *), Bash(gh auth status), Bash(git push *), Read
---

# Provision Research-Space Workspace

Creates a new workspace for open-ended research. This plugin's primitives (`/research-space:source-log`, `/research-space:summarize-sources`, `/research-space:deep-dive`, etc.) are globally available once installed — this skill only provisions the **data scaffold** (CLAUDE.md, context/, sources/, outputs/, notes/) that those commands read from and write to.

## Arguments

`$ARGUMENTS` is parsed as:

- **First positional**: workspace name (kebab-case). Required.
- **Second positional** (optional): target parent path. Defaults to `~/repos/github/my-repos`.
- **`--variant=<variant>`** (optional): which scaffold to copy. Default: `deep-research`. Supported: `deep-research`, `technical`, `osint`, `georeaction`, `stack`, `ecosystem`, `competitor`.
- **`--local-only`** (optional): skip GitHub repo creation and push. Default: create a public GitHub repo and push.
- **`--private`** (optional): create the GitHub repo as private. Default: public.

### Examples

```
/research-space:new-workspace iran-drone-program --variant=osint
/research-space:new-workspace acme-competitor --variant=competitor
/research-space:new-workspace local-ai-stack --variant=stack --local-only
/research-space:new-workspace llm-ecosystem-2026 --variant=ecosystem
```

## Procedure

### 1. Parse arguments

Extract workspace name, target parent path, variant, and flags from `$ARGUMENTS`. If workspace name is missing, ask the user before proceeding. If variant is not one of the supported seven, list the supported ones and stop.

### 2. Resolve the scaffold path

The bundled scaffold lives at `${CLAUDE_SKILL_DIR}/../../template/<variant>/`. Confirm it exists before copying.

### 3. Read ambient facts

Read `~/.claude/CLAUDE.md` if it exists. Extract OS, locale, timezone, and user identity facts. These will personalise the workspace's CLAUDE.md at step 5.

### 4. Create the workspace directory

```bash
mkdir -p <target-parent>/<workspace-name>
cp -r ${CLAUDE_SKILL_DIR}/../../template/<variant>/. <target-parent>/<workspace-name>/
```

Do **not** copy any `.claude/` tree. The plugin's primitives are global.

### 5. Personalise CLAUDE.md

Open the new workspace's `CLAUDE.md` and:

- Replace placeholder identity/locale with facts from step 3.
- Insert a header with workspace name, variant, and date.
- If the variant has a research-subject placeholder (e.g. `<SUBJECT>` in competitor/single-company scaffolds), prompt the user and fill it in.

### 6. Prompt for workspace-specific facts

Ask only for facts the plugin can't infer:

- **deep-research / technical**: the central research question.
- **osint**: the investigation subject and (optionally) the geographic scope.
- **georeaction**: the event or policy and the regions being compared.
- **stack**: the technology area under review.
- **ecosystem**: the ecosystem being mapped (geography or sector).
- **competitor**: the competitor company name and your own company for framing.

Write these into `CLAUDE.md` or `context/scope.md` as appropriate.

### 7. Initialise git and (optionally) publish

```bash
cd <target-parent>/<workspace-name>
git init
git add .
git commit -m "Initial workspace from research-space plugin (<variant>)"
```

Unless `--local-only` is set:

```bash
gh repo create <workspace-name> --<public|private> --source=. --push
```

### 8. Print next steps

Tell the user:

- Workspace path and variant.
- First commands to run: `/research-space:research-init`, then `/research-space:source-log <url>` as they gather material.
- When they have a handful of sources: `/research-space:summarize-sources`.
- For focused investigation: `/research-space:deep-dive <topic>`.
- Reminder that the workspace is **data** — they can delete/move it freely without losing plugin commands.

## Notes

- Resolve the scaffold via `${CLAUDE_SKILL_DIR}/../../template/` (not `${CLAUDE_PLUGIN_ROOT}`).
- Never copy `.claude/commands/`, `.claude/agents/`, or `.claude/skills/` into the new workspace.
- Don't hard-code personal paths or identifiers — everything comes from user memory or prompts.
