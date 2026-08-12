# Obsidian vault mechanics for agent-written vaults

What the `obsidian-vault` variant encodes, and why each choice is the way it is. Written for whoever next changes `template/obsidian-vault/` — or builds another agent-facing vault and would otherwise rediscover this.

**Verified:** 2026-08-12, against Obsidian's documented config format. Marked below where a claim is **confirmed** from the shipped scaffold and where it is **inferred** from Obsidian behaviour and not re-tested in a live app on this machine.

## The problem this variant solves

An agent writing markdown into folders produces a directory that Obsidian will happily open and that is, inside Obsidian, useless: no graph, no backlinks, no properties, no filters. Every Obsidian affordance is downstream of two things and only two things — **YAML frontmatter** and **`[[wikilinks]]`**. A vault is not a folder layout; it is a link graph with typed nodes. That is the whole design constraint.

So the scaffold's job is not to make folders. It is to make the frontmatter schema and the link discipline non-optional for the agent, which is what `CLAUDE.md` and `Meta/Conventions.md` do, and to ship the Obsidian-side config that makes those pay off immediately.

## What makes a folder a vault

**Confirmed.** `.obsidian/` existing in a directory is what makes Obsidian treat it as a vault. Committing that directory is what makes `git clone` → `Open folder as vault` land on a configured vault rather than a default one. The scaffold ships:

| File | Carries |
|------|---------|
| `app.json` | Editor and file-handling behaviour |
| `appearance.json` | Theme, accent colour |
| `core-plugins.json` | Which built-in plugins are on |
| `graph.json` | Graph view settings, including colour groups |
| `templates.json` | Templates-plugin folder and date formats |

`workspace.json` is deliberately **not** shipped and is gitignored: it is per-machine UI layout (open panes, tab positions, window geometry) and conflicts on essentially every pull between two machines. Same for `workspace-mobile.json` and `.obsidian/plugins/*/data.json`.

### Settings that matter, and why

- **`alwaysUpdateLinks: true`** — Obsidian rewrites backlinks when *it* performs a rename. This is the single most important setting in an agent-shared vault, and it is also the one with the sharpest limit: it does nothing for a rename performed from the shell, from `git mv`, or by an agent's file tools. The scaffold therefore also tells the agent, in `CLAUDE.md`, to `rg -l "\[\[Old Name"` and rewrite references by hand. Both halves are needed; neither covers the other's case.
- **`useMarkdownLinks: false`** + **`newLinkFormat: "shortest"`** — keeps links as `[[Note title]]` rather than `[Note](Note.md)`. Wikilinks are what an agent can grep for reliably and what Obsidian's aliasing works with.
- **`attachmentFolderPath: "Assets"`** — otherwise pasted images scatter next to whatever note was focused, and the vault accumulates binaries in content folders.
- **`newFileLocation: "folder"` + `newFileFolderPath: "Notes"`** — a stray `Ctrl+N` lands in scratch, not in `Sources/`.

## Frontmatter: the one syntax trap

**Confirmed** from Obsidian's properties documentation, and the reason `Meta/Conventions.md` spells it out: a wikilink inside frontmatter renders as a link **only when quoted**.

```yaml
sources: ["[[240812-acme-filing]]"]   # ✅ link
sources: [[[240812-acme-filing]]]     # ❌ invalid YAML
sources: [[240812-acme-filing]]       # ❌ valid YAML, nested list, no link
```

The second form is what an agent writes if left to its own devices, and it fails as a YAML parse — which Obsidian surfaces as a broken properties block rather than as an error anyone would connect to the cause. Worth stating explicitly in any agent instruction file for a vault.

The schema itself (`type`, plus per-type fields) is shaped so one property — `type` — is the primary filter for every view. That makes the same schema serve Dataview queries, Obsidian's built-in Bases, and plain search, without three vocabularies.

## Not depending on community plugins

Dataview is close to universal in Obsidian circles but is still a community plugin, and a scaffold that only works after installing one is a scaffold that fails on first open. The variant resolves this by maintaining **both**:

- a hand-written map of content per folder (`Questions/Questions.md`, …), which the agent updates as it adds notes — works on a fresh install;
- `Meta/Dataview Queries.md`, the same views as live queries — works if Dataview is present, and degrades to inert code blocks if not.

Cost is real: the agent must remember to update the MOCs. `CLAUDE.md` carries that instruction under *Maintaining the indexes*.

**Bases** (Obsidian's built-in database view) would cover much of the Dataview ground with no community plugin. No `.base` file ships, deliberately — the file format was not verified against a live Obsidian on this machine, and shipping a guessed schema that fails to load is worse than shipping nothing. The frontmatter is already shaped for it. **Inferred / unverified:** that a hand-written `.base` would load cleanly. Build one against your own Obsidian version before adding it here.

## Canvas format

**Confirmed** by construction and JSON-validated; **inferred** as to Obsidian's tolerance of the optional fields. `.canvas` is plain JSON:

```json
{
  "nodes": [
    {"id": "...", "type": "file", "file": "Findings/Findings.md",
     "x": 0, "y": 0, "width": 340, "height": 260, "color": "4"},
    {"id": "...", "type": "text", "text": "markdown",
     "x": 0, "y": 0, "width": 640, "height": 200, "color": "6"}
  ],
  "edges": [
    {"id": "...", "fromNode": "...", "fromSide": "bottom",
     "toNode": "...", "toSide": "top", "label": "assembled into"}
  ]
}
```

- `color` is a preset string `"1"`–`"6"` (red, orange, yellow, green, cyan, purple) — **not** a hex value. Hex is accepted in the app's colour picker but the preset strings are what survive round-tripping.
- Node `file` paths are vault-relative, including the extension.
- `y` increases downward.
- A node whose `file` does not exist renders as an empty card rather than an error — so a broken path is silent. Re-validate with `python3 -m json.tool` after any scripted edit, and check the paths separately.

Graph-view `colorGroups` uses a different colour encoding again — `{"a": 1, "rgb": <int>}`, where the int is `r<<16 | g<<8 | b`. Two colour formats in one config tree; do not copy one into the other.

## HTML comments do not hide wikilinks

**Confirmed** by building the scaffold and scanning it: Obsidian indexes `[[links]]` inside `<!-- HTML comments -->` exactly as it does anywhere else. An illustrative `<!-- - [[Note title]] — one line on what it says -->` in a map-of-content note becomes a real unresolved node in the graph, and seven such comments produce seven ghost nodes on first open.

The first cut of this scaffold shipped exactly that mistake, and its own `CLAUDE.md` forbade it. Examples must go in backticks (code spans are not indexed) or be written out in words. Worth a scan before shipping any vault:

```bash
# unresolved link targets, ignoring fenced blocks and code spans
python3 - <<'EOF'
import pathlib, re
md=[p for p in pathlib.Path('.').rglob('*.md') if '.git' not in p.parts]
stems={p.stem for p in md}
for p in md:
    t=re.sub(r'`[^`\n]*`','',re.sub(r'```.*?```','',p.read_text(),flags=re.S))
    for m in re.finditer(r'!?\[\[([^\]|#]+)',t):
        if m.group(1).strip() not in stems: print(p, '->', m.group(1))
EOF
```

## Templates: two templating systems in one folder

`Templates/*.md` are consumed by **both** Obsidian's core Templates plugin and the agent. They contain `{{date}}` tokens, which are Obsidian's syntax, filled at note-creation time.

The provisioning skill substitutes `<WORKSPACE_NAME>`, `<RESEARCH_QUESTION>` and `<DATE>` across the scaffold — and must **not** touch `{{date}}`. Two placeholder syntaxes coexisting in one tree is a trap; the skill's step 6a says so explicitly for exactly that reason.

`{{date}}` is left **unquoted** in template frontmatter, which is a deliberate trade:

- Unquoted → the created note gets `created: 2026-08-12`, which Dataview and Bases treat as a date, so `WHERE updated >= date(today) - dur(7 days)` works. But the template file itself has frontmatter that is not valid YAML — `{{date}}` parses as a mapping key and PyYAML rejects it with *"found unhashable key"*.
- Quoted (`"{{date}}"`) → the template file parses cleanly, but every created note carries `created: "2026-08-12"` as a **string**, and every date comparison in `Meta/Dataview Queries.md` silently stops matching.

Silent query failure is worse than a warning on six files nobody opens, so: unquoted, and `.obsidian/app.json` sets `userIgnoreFilters: ["Templates/"]` to keep the folder out of search, graph, backlinks and the quick switcher entirely. Templates are inserted through the Templates plugin's own command, which does not honour that filter.

## Copying the scaffold

`cp -r template/obsidian-vault/. <dest>/` — the trailing `/.` is load-bearing. Without it, `.obsidian/` and `.gitignore` do not come across and the result is a folder of markdown that Obsidian will open as an unconfigured vault. The failure is quiet: everything looks fine until the graph is grey and the properties panel is empty. The skill verifies `.obsidian/app.json` exists after the copy.

## Nested `.gitignore`

The scaffold ships its own `.gitignore`, which sits inside this plugin repo at `template/obsidian-vault/.gitignore` and is therefore honoured by *this* repo too. It currently ignores `Outputs/**/*.pdf` and `Outputs/**/*.mp3` — harmless here, since the template ships no outputs, but worth knowing before adding fixture files under that path and wondering why git does not see them.
