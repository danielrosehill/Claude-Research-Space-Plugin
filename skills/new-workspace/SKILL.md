---
name: new-workspace
description: Provision a new research-space workspace on disk. Use when the user wants to start a new research project (deep-research, technical, osint, georeaction, stack, ecosystem, competitor, purchasing, general-research-workspace, or obsidian-vault). Accepts a workspace name and optional variant. Scaffolds the workspace, personalises CLAUDE.md from the user's global memory, creates a GitHub repo (public by default), and auto-registers public workspaces into the Open-Research-Workspaces-Index.
disable-model-invocation: true
allowed-tools: Bash(mkdir *), Bash(cp *), Bash(cat *), Bash(git init *), Bash(git add *), Bash(git commit *), Bash(gh repo create *), Bash(gh auth status), Bash(git push *), Bash(gh repo clone *), Bash(git clone *), Bash(git -C *), Bash(rm -rf /tmp/*), Read, Edit
---

# Provision Research-Space Workspace

Creates a new workspace for open-ended research. This plugin's primitives (`/research-space:source-log`, `/research-space:summarize-sources`, `/research-space:deep-dive`, etc.) are globally available once installed — this skill only provisions the **data scaffold** (CLAUDE.md, context/, sources/, outputs/, notes/, plus variant-specific folders) that those commands read from and write to.

## Arguments

`$ARGUMENTS` is parsed as:

- **First positional**: workspace name (Train-Case preferred, kebab-case accepted). Required.
- **Second positional** (optional): target parent path. Defaults to `~/repos/github/my-repos`.
- **`--variant=<variant>`** (optional): which scaffold to copy. Default: `deep-research`. Supported:
  - `deep-research` — general-purpose investigation around one central question
  - `technical` — version-sensitive technical research (APIs, libraries, protocols, hardware)
  - `osint` — open-source intelligence with provenance and chain-of-custody
  - `georeaction` — comparative regional reactions to an event or policy
  - `stack` — tool/stack evaluation with criteria and scoring
  - `ecosystem` — actor/landscape mapping
  - `competitor` — competitor intelligence
  - `purchasing` — spec → candidates → shortlist → decision for a purchase
  - `general-research-workspace` — openly-logged Q&A research space (user asks, Claude writes longform responses, pairs consolidated into PDFs)
  - `obsidian-vault` — the standard research loop scaffolded as a working Obsidian vault (committed `.obsidian/` config, frontmatter schema, wikilinks, note templates, canvas). Pick this when the user says "Obsidian", "vault", "PKM", or wants to browse the research by graph and backlinks rather than by folder
- **`--local-only`** (optional): skip GitHub repo creation and push. Default: create a GitHub repo and push.
- **`--private`** (optional): create the GitHub repo as private. Default: public. Private workspaces are **not** auto-registered into the public index.
- **`--no-index`** (optional): even for a public workspace, skip the auto-registration into `Open-Research-Workspaces-Index`.

### Examples

```
/research-space:new-workspace Iran-Drone-Program --variant=osint
/research-space:new-workspace Acme-Competitor --variant=competitor
/research-space:new-workspace Local-AI-Stack --variant=stack --local-only
/research-space:new-workspace LLM-Ecosystem-2026 --variant=ecosystem
/research-space:new-workspace Framework-Laptop-Purchase --variant=purchasing --private
/research-space:new-workspace State-Of-Claude-Context-0426 --variant=general-research-workspace
/research-space:new-workspace Agent-Memory-Landscape --variant=obsidian-vault
```

## Procedure

### 1. Parse arguments

Extract workspace name, target parent path, variant, and flags from `$ARGUMENTS`. If workspace name is missing, ask the user before proceeding. If variant is not one of the supported ten, list the supported ones and stop.

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

The trailing `/.` matters: it carries dotfiles. For `obsidian-vault` that is the whole point — without `.obsidian/` and `.gitignore` the result is a folder of markdown, not a vault. After copying that variant, verify:

```bash
test -f <workspace>/.obsidian/app.json && test -f <workspace>/.gitignore || echo "dotfiles missing — re-copy with the trailing /."
```

### 5. Personalise CLAUDE.md

Open the new workspace's `CLAUDE.md` and:

- Replace placeholder identity/locale with facts from step 3.
- Insert a header with workspace name, variant, and date.
- Fill in any variant-specific placeholder (see step 6).

### 6. Prompt for workspace-specific facts

Ask only for facts the plugin can't infer:

- **deep-research / technical**: the central research question → `<RESEARCH_QUESTION>`.
- **osint**: the investigation subject and (optionally) the geographic scope.
- **georeaction**: the event or policy and the regions being compared.
- **stack**: the technology area under review → `<STACK_AREA>`.
- **ecosystem**: the ecosystem being mapped (geography or sector).
- **competitor**: the competitor company name and your own company for framing.
- **purchasing**: the purchase target (what's being bought) → `<PURCHASE_TARGET>`. Offer to seed `context/spec.md` with a short intake.
- **general-research-workspace**: the research question or theme → `<RESEARCH_QUESTION>`. This is the umbrella question; individual questions accrue under `questions/`.
- **obsidian-vault**: the central question → `<RESEARCH_QUESTION>`, plus the topic slug for the `#research/<topic>` tag.

Write these into `CLAUDE.md` or `context/scope.md` as appropriate.

### 6a. `obsidian-vault` only — substitute placeholders and seed the tag

This variant carries placeholders in more files than the others, because the vault's own notes are part of the scaffold. Replace across every `.md` and the `.canvas`:

| Placeholder | Replace with |
|-------------|--------------|
| `<WORKSPACE_NAME>` | the workspace name |
| `<RESEARCH_QUESTION>` | the central question |
| `<DATE>` | today's date, ISO (`YYYY-MM-DD`) |

```bash
cd <workspace>
grep -rl '<WORKSPACE_NAME>\|<RESEARCH_QUESTION>\|<DATE>' . --include='*.md' --include='*.canvas'
```

Substitute in each, then confirm none remain. `Research Map.canvas` is JSON — edit the string values only, and re-validate with `python3 -m json.tool` afterwards.

Then set the topic tag: replace the empty `research/` tag in `Meta/Tags.md`, `Home.md`, `Context/Scope.md` and the six files in `Templates/` with `research/<topic>`, so notes created from the templates carry it from the start.

Leave the `{{date}}` tokens in `Templates/` alone — those are Obsidian's own Templates-plugin syntax, filled at note-creation time, not now.

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

### 8. Auto-register into Open-Research-Workspaces-Index

**Skip this step if any of:** `--local-only`, `--private`, `--no-index`.

The public index lives at `github.com/danielrosehill/Open-Research-Workspaces-Index`. Append a new entry to its `README.md` under the `## Research Workspaces` section so the new workspace is discoverable.

Procedure:

1. Clone the index into a temp dir:
   ```bash
   TMP=$(mktemp -d)
   gh repo clone danielrosehill/Open-Research-Workspaces-Index "$TMP/index"
   ```
2. Edit `$TMP/index/README.md`. Insert the following block **immediately before** the `---` that closes the `## Research Workspaces` section (i.e. append as the last workspace entry, before the horizontal rule that precedes `## Templates`):

   ```markdown
   ### <Human-readable title derived from workspace name>

   <One-paragraph description>. **Question:** <the central question / umbrella theme>.

   [![View Repo](https://img.shields.io/badge/View-Repo-blue?style=flat&logo=github)](https://github.com/danielrosehill/<workspace-name>)
   ```

   Also bump the `**Last Updated:** <Month YYYY>` line to the current month/year if it's out of date.

3. Commit and push:
   ```bash
   git -C "$TMP/index" add README.md
   git -C "$TMP/index" commit -m "Add <workspace-name> to research workspaces index"
   git -C "$TMP/index" push
   ```
4. Clean up: `rm -rf "$TMP"`.

If the insertion fails (section markers changed, merge conflict, etc.), report it and let the user handle manually — do not force-push.

### 9. Print next steps

Tell the user:

- Workspace path, variant, and GitHub URL.
- Whether the workspace was added to `Open-Research-Workspaces-Index` (and the link), or why it was skipped.
- First commands to run: `/research-space:research-init`, then `/research-space:source-log <url>` as they gather material.
- When they have a handful of sources: `/research-space:summarize-sources`.
- For focused investigation: `/research-space:deep-dive <topic>`.
- For `general-research-workspace`: note the Q&A pair convention (`questions/<slug>.md` + `answers/<slug>.md`) and that `/research-space:export-report` consolidates selected pairs into a single doc.
- For `obsidian-vault`: tell them to open the folder in Obsidian (`Open folder as vault`) — `.obsidian/` is committed, so it opens configured — and to start at `Home.md` or `Research Map.canvas`. Point them at `Meta/Conventions.md` for the frontmatter schema. Mention that no community plugins are needed, and that `Meta/Dataview Queries.md` becomes live if they have Dataview.
- Reminder that the workspace is **data** — they can delete/move it freely without losing plugin commands.

## Notes

- Resolve the scaffold via `${CLAUDE_SKILL_DIR}/../../template/` (not `${CLAUDE_PLUGIN_ROOT}`).
- Never copy `.claude/commands/`, `.claude/agents/`, or `.claude/skills/` into the new workspace.
- Don't hard-code personal paths or identifiers — everything comes from user memory or prompts.
- Prefer Train-Case repo names (`State-Of-Claude-Context-0426`) over kebab-case when publishing publicly — matches Daniel's convention.
- The index registration step is intentionally idempotent-friendly: if the entry already exists (detect by URL substring), skip the insert and just push a no-op log.
