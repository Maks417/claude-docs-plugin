---
description: Create or update the project's docs/ folder (index, architecture, domain model, brandbook, per-feature docs). Wraps the docs-keeper skill.
allowed-tools: Read, Write, Edit, Glob, Grep, Bash
---

Follow the **docs-keeper** skill for the full workflow, templates, and conventions. Parse
`$ARGUMENTS` to choose the intent:

- **`init`** — Locate or create the docs directory, then generate the four core docs:
  `index.md`, `architecture.md`, `domain-model.md`, `brandbook.md`. Derive everything from
  the actual codebase.
- **`feature <name>`** — Create or update `docs/features/<kebab-name>.md` for the named
  feature, then update the link table in `index.md`.
- **`architecture`** — Refresh only `docs/architecture.md`.
- **`domain`** — Refresh only `docs/domain-model.md`.
- **`brand`** — Refresh only `docs/brandbook.md`.
- **`legal`** — Refresh only `docs/legal.md` (GDPR and other regulatory posture).
- **`owasp`** — Refresh only `docs/owasp.md` (security posture mapped to the OWASP Top 10).
- **`sync`** — Re-derive the link tables in `docs/index.md` from the docs that actually
  exist on disk (add missing links, remove dead ones).
- **no arguments** — Summarize the current state of the docs directory (what exists, what's
  missing, any `_TBD_` sections) and suggest the next action. Do not write files.

Arguments: `$ARGUMENTS`

Always reuse an existing docs directory (`docs/` or `doc/`) rather than creating a
duplicate. All diagrams must be Mermaid.
