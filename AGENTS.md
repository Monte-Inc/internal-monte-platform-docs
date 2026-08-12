# Documentation project instructions

## About this project

- This is a documentation site built on [Mintlify](https://mintlify.com)
- Pages are MDX files with YAML frontmatter
- Configuration lives in `docs.json`
- Use the Mintlify MCP server, `https://mcp.mintlify.com`, to edit content and settings via MCP
- Use the Mintlify docs MCP server, `https://www.mintlify.com/docs/mcp`, to query information about using Mintlify via MCP

## Terminology

Monte's vocabulary is strictly defined in `monte-nemo-platform/CONTEXT.md` — use it exactly:

- **Experiment** — a high-level investigation that groups related Runs around one objective. It uses one Measurement by default but may use more.
- **Measurement** — a frozen evaluation contract over one Environment. Capitalize. Never "benchmark" or "eval suite".
- **Environment** — a pluggable graded task package. Never "dataset".
- **Run** — one eval or train execution that originates in exactly one Experiment and is recorded in a Measurement's Ledger. Training data, recipes, and hyperparameters bind here.
- **Lineage** — a chain of training runs building on one another.
- **Ledger** — the append-only per-Measurement record. Never "database" or "history".
- **Split** — a frozen task partition (train pool, dev, test).
- **Baseline** — the base-model eval later runs are compared against.
- The CLI is `monte` (code-formatted, lowercase).

## Style preferences

{/* Add any project-specific style rules below */}

- Use active voice and second person ("you")
- Keep sentences concise — one idea per sentence
- Use sentence case for headings
- Bold for UI elements: Click **Settings**
- Code formatting for file names, commands, paths, and code references

## Content boundaries

- Document the user-facing surfaces: CLI, environments, console, concepts.
- Do not copy internal planning docs, research reports, or box operational details (hostnames, IPs, SSH specifics) into this site.
- The platform repo's `docs/` folder is internal engineering material, not a source to republish verbatim.
