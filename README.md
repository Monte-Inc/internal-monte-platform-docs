# Monte Platform docs

Documentation site for the Monte platform (`monte-nemo-platform`), built on [Mintlify](https://mintlify.com). Pages are MDX with YAML frontmatter; navigation and theme live in `docs.json`.

Current state: full content pass done (2026-08-10) — every page carries real prose, written against the code in `monte-nemo-platform` and verified against it. Style follows ASD-STE100 flavored mode (the `ste-writing` skill): short active sentences, one name per thing, linted with `ste-lint.py`. The CLI reference is hand-written for now; a `gen-cli-docs.py` generator from the Typer app remains a possible later step.

## Local preview

```
npm i -g mint
mint dev
```

Run from the repo root (where `docs.json` is). Preview at `http://localhost:3000`.

## Publishing

Pushes to `main` deploy automatically via the Mintlify GitHub App (connected in the [dashboard](https://dashboard.mintlify.com)).

## Conventions

See `AGENTS.md` for terminology (Measurement, Environment, Run, Dataset, Source, Move, Cohort, Lineage, Recipe, Ledger, Vault) and content boundaries. Vocabulary is defined in `monte-nemo-platform/CONTEXT.md`.

## Troubleshooting

- Dev server issues: `mint update` to get the latest CLI.
- Page 404s locally: make sure you're running in the folder containing `docs.json`.
