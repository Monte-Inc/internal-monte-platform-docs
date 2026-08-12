# Documentation project instructions

## About this project

- This is a documentation site built on [Mintlify](https://mintlify.com)
- Pages are MDX files with YAML frontmatter
- Configuration lives in `docs.json`
- Use the Mintlify MCP server, `https://mcp.mintlify.com`, to edit content and settings via MCP
- Use the Mintlify docs MCP server, `https://www.mintlify.com/docs/mcp`, to query information about using Mintlify via MCP

## Terminology

Monte's vocabulary is strictly defined in `monte-nemo-platform/CONTEXT.md` — use it exactly:

- **Experiment** — RESERVED, not implemented. A high-level investigation that groups related Runs around one objective. A person declares one, so it is never derived — that is what separates it from Cohort and Lineage. No code creates, stores, or reads one. Do not document it as something a reader can use.
- **Measurement** — a frozen evaluation contract over one Environment. Capitalize. Never "benchmark" or "eval suite".
- **Environment** — a pluggable graded task package. Never "dataset".
- **Run** — one eval or train execution, recorded as a row in a Measurement's Ledger. The model and the Recipe bind here.
- **Cohort** — the group of Runs sharing a base model and comparability config: the set within which a number means something. One Baseline each; deltas never cross one. Derived from row fields, stored nowhere.
- **Lineage** — a Checkpoint's descent path: base model → Checkpoint → Checkpoint, each hop labelled by the Recipe that produced it. Also derived. Checkpoints form a tree, not a line.
- **Recipe** — the versioned file stating *how* a training Run trains (model shape, algorithm, batch geometry, optimizer). Binds per Run, defaults from the Measurement, pinned by content hash.
- **Ledger** — the append-only per-Measurement record. Never "database" or "history".
- **Split** — a frozen task partition (train pool, dev, test).
- **Baseline** — the base-model eval later runs are compared against.
- **chain** is a **verb**, never a noun: "a Run chains from a Checkpoint". There is no `Chain` object.
- The CLI is `monte` (code-formatted, lowercase).

Until 2026-08-11 the platform used **Lineage** for what is now **Cohort**.
Both words are live and mean different things: a Cohort is a set (what is
comparable), a Lineage is a path (what came from what). Applied across this
site 2026-08-12 — if you find a page still calling the comparability group a
Lineage, it was missed and should be fixed.

### Not yet true of the platform

Keep this list short and delete entries as they land — a stale "coming soon"
is worse than no note at all.

- **Staged training.** A Run may not yet start from a Checkpoint you name:
  there is no `--parent` or `--from-base`, so a chunk always continues its
  Cohort's newest Checkpoint, and a Recipe change on that path is refused.
  Until it lands, do not document branching as something a reader can do.
  (Platform: `docs/adr/0002-staged-training-is-explicit.md`.)

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
