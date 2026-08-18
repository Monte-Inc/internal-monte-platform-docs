# Documentation project instructions

## About this project

- This is a documentation site built on [Mintlify](https://mintlify.com)
- Pages are MDX files with YAML frontmatter
- Configuration lives in `docs.json`
- Use the Mintlify MCP server, `https://mcp.mintlify.com`, to edit content and settings via MCP
- Use the Mintlify docs MCP server, `https://www.mintlify.com/docs/mcp`, to query information about using Mintlify via MCP

## Terminology

Monte's vocabulary is strictly defined in `monte-nemo-platform/CONTEXT.md` — use it exactly:

- **experiment** (lowercase) — a Measurement over a trainable Environment: one training story whose evidence is self-contained. The CLI verb is `monte experiment init`. Its counterpart is a Measurement over an eval-only Environment: a long-lived reference any model can be measured against. The platform's `CONTEXT.md` calls that a **yardstick**, but this site does not use the word — write "an eval-only Measurement" instead. Capital-E **Experiment** (a grouping above Measurements) stays RESERVED and unimplemented — do not document that one as usable.
- **Measurement** — a frozen evaluation contract over one Environment. Capitalize. Never "benchmark" or "eval suite".
- **Environment** — a pluggable graded task package. Never "dataset" (see Dataset below — a different thing).
- **Run** — one eval or train execution, recorded as a row in a Measurement's Ledger. The model, the Recipe, and the Source bind here.
- **Source** — what a training Run trains on, stated on every train (`--source`): a trainable Environment's curriculum (its own or another's) or a frozen Dataset (sft only). Recorded on the row; rendered in the hop label `algorithm:source`.
- **Dataset** (capital D) — a frozen, named pile of demonstration rows for sft (`monte dataset add`), with any overlap against frozen exams recorded at freeze. Never part of a Measurement's definition. The lowercase ban above still stands.
- **Vault** (2026-08-18, ADR 0006) — the cloud bucket that `monte env add` and `monte dataset add` push what they freeze to. Capitalize it, as `CONTEXT.md` does. It is the source of truth for what reached it, and the copy under a data root is a **materialization** — never a Mirror, which means results only. An add with no writable GCS key freezes locally and pushes nothing, and an Environment shipped as a package never enters the Vault at all, so "every frozen input is in the Vault" is not a claim to make. There are two cloud buckets now, so name the one you mean: write "the Vault" or "the results bucket". The old singular, "the durable tier", named one bucket back when there was one and no longer says which. Applied across this site 2026-08-18.
- **Move** — one decision by the driving agent: a Checkpoint × a Source × a Recipe.
- **Cohort** — the group of Runs sharing a base model and comparability config: the set within which a number means something. One Baseline each; deltas never cross one. Derived from row fields, stored nowhere.
- **Lineage** — a Checkpoint's descent path: base model → Checkpoint → Checkpoint, each hop labelled by its move (`algorithm:source`); the Recipe stays on the row. Also derived. Checkpoints form a tree, not a line.
- **Recipe** — the versioned file stating *how* a training Run trains (model shape, algorithm, batch geometry, optimizer). Binds per Run, inherits the pinned trainer's shipped config, pinned by content hash.
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

- **The Vault** — `monte env list --vault`, `monte dataset list --vault`, the
  push on `env add` / `dataset add`, and `monte env install` fetching from the
  Vault all live on the platform's `vault-core` branch, not yet on its `main`.
  This site describes them as built. Delete this entry when `vault-core`
  merges, and do not merge the docs change before it.
- (the checkpoint tree, `--parent`/`--from-base`, the `monte status` tree
  render, and the source axis all shipped as of 2026-08-17. The Concepts
  pages' branching content is now a description of built behavior.)

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
