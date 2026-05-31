# docs-keeper

A Claude Code plugin that creates and maintains a consistent set of project documentation
under a `docs/` folder. It ships a **subagent** (`docs-keeper`) and a **`/docs` slash
command**, and works in any project on any machine.

## What it produces

In every project, under `docs/` (it reuses an existing `docs/` or `doc/` folder if one
already exists):

| Doc | Contents |
|---|---|
| `index.md` | Short project description + a link table to every other doc |
| `architecture.md` | Components, responsibilities, data flow + a Mermaid diagram |
| `domain-model.md` | Entities, relationships, invariants + a Mermaid ER diagram |
| `brandbook.md` | Brand voice, color palette, typography, tokens, components |
| `features/<name>.md` | One doc per major feature |

All diagrams use [Mermaid](https://mermaid.js.org/), which renders on GitHub and in most
markdown viewers.

## Install

On any machine:

```
/plugin marketplace add <git-url-or-local-path-to-this-repo>
/plugin install docs-keeper@docs-tools
```

`docs-tools` is the marketplace name; `docs-keeper` is the plugin. For local development
you can point at the checkout directly, e.g.
`/plugin marketplace add C:\Job\claude-docs-plugin`.

To share across machines, push this repo to a Git host and `marketplace add` the URL. Pull
changes with `git pull`, then `/plugin update docs-keeper` to pick them up.

## Usage

Via the slash command:

| Command | Action |
|---|---|
| `/docs init` | Set up `docs/` and generate the four core docs |
| `/docs feature <name>` | Create/update `docs/features/<name>.md` |
| `/docs architecture` | Refresh the architecture doc |
| `/docs domain` | Refresh the domain-model doc |
| `/docs brand` | Refresh the brandbook |
| `/docs sync` | Rebuild the `index.md` link tables from what's on disk |
| `/docs` | Report the current docs state and suggest the next step |

Or just ask in natural language — e.g. "document this project" or "add a feature doc for
billing" — and Claude will delegate to the `docs-keeper` agent.

## Notes

- The agent derives content from the actual codebase and marks genuinely unknown sections
  `_TBD_` rather than guessing.
- Editing an existing doc updates it in place; the `index.md` link tables are kept in sync.
