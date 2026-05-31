# docs-keeper

Create and maintain a consistent set of project documentation under a `docs/` folder: an
index, an architecture doc, a domain-model doc, a brandbook/design system, and one doc per
major feature. All diagrams use [Mermaid](https://mermaid.js.org/).

The logic lives in a single **Agent Skill** (`SKILL.md`) so it works on both surfaces:

- **Claude Code** (CLI, IDE extensions, Claude Code desktop app) — installed as a plugin;
  the skill is auto-available and a `/docs` slash command wraps it.
- **Claude Desktop** (the Claude app) — the same skill folder is added under
  **Settings → Capabilities → Skills**.

## What it produces

In every project, under `docs/` (it reuses an existing `docs/` or `doc/` folder if present):

| Doc | Contents |
|---|---|
| `index.md` | Short project description + a link table to every other doc |
| `architecture.md` | Components, responsibilities, data flow + a Mermaid diagram |
| `domain-model.md` | Entities, relationships, invariants + a Mermaid ER diagram |
| `brandbook.md` | Brand voice, color palette, typography, tokens, components |
| `features/<name>.md` | One doc per major feature |

## Install in Claude Code

```
/plugin marketplace add https://github.com/Maks417/claude-docs-plugin
/plugin install docs-keeper@docs-tools
```

`docs-tools` is the marketplace name; `docs-keeper` is the plugin. `/plugin` is typed at
the interactive Claude Code prompt (terminal `claude`, the IDE extension, or the Claude
Code desktop app) — not in a shell. For local development you can point at a checkout:
`/plugin marketplace add C:\Job\claude-docs-plugin`.

Updates: `git pull` (or it auto-updates), then `/plugin update docs-keeper`.

### Usage (Claude Code)

| Command | Action |
|---|---|
| `/docs init` | Set up `docs/` and generate the four core docs |
| `/docs feature <name>` | Create/update `docs/features/<name>.md` |
| `/docs architecture` | Refresh the architecture doc |
| `/docs domain` | Refresh the domain-model doc |
| `/docs brand` | Refresh the brandbook |
| `/docs sync` | Rebuild the `index.md` link tables from what's on disk |
| `/docs` | Report the current docs state and suggest the next step |

Or just ask in natural language — "document this project", "add a feature doc for billing".

## Install in Claude Desktop

Claude Desktop doesn't load Claude Code plugins, but it supports the same skill:

1. Open **Settings → Capabilities → Skills** and choose to add a skill.
2. Upload `dist/docs-keeper-skill.zip` (or zip the `plugins/docs-keeper/skills/docs-keeper/`
   folder yourself — the zip must contain `SKILL.md` at its root).
3. In a chat, ask to "document this project" or "write the architecture doc".

To write files into a real repo from Claude Desktop, enable a filesystem connector (MCP) or
work inside a Project with file access. Without that, the skill outputs each document's
Markdown in the conversation, labeled by file path, for you to save into `docs/` yourself.

## Repo layout

```
.claude-plugin/marketplace.json          marketplace "docs-tools"
plugins/docs-keeper/
  .claude-plugin/plugin.json             plugin manifest
  commands/docs.md                       /docs command (Claude Code)
  skills/docs-keeper/SKILL.md            the portable skill (both surfaces)
dist/docs-keeper-skill.zip               zipped skill for Claude Desktop upload
```

## Notes

- Content is derived from the actual codebase; genuinely unknown sections are marked `_TBD_`
  rather than guessed.
- Editing an existing doc updates it in place; the `index.md` link tables are kept in sync.
