# Monte Platform docs

Documentation site for the Monte platform (`monte-nemo-platform`), built on [Mintlify](https://mintlify.com). Pages are MDX with YAML frontmatter; navigation and theme live in `docs.json`.

Current state: skeleton only — every page is a section outline (headings + bullets), no prose yet. The CLI reference pages will be generated from the Typer app in `monte-nemo-platform` (`scripts/gen-cli-docs.py`), not hand-written.

## Local preview

```
npm i -g mint
mint dev
```

Run from the repo root (where `docs.json` is). Preview at `http://localhost:3000`.

## Publishing

Pushes to `main` deploy automatically via the Mintlify GitHub App (connected in the [dashboard](https://dashboard.mintlify.com)).

## Conventions

See `AGENTS.md` for terminology (Measurement, Environment, Run, Lineage, Ledger) and content boundaries. Vocabulary is defined in `monte-nemo-platform/CONTEXT.md`.

## Troubleshooting

- Dev server issues: `mint update` to get the latest CLI.
- Page 404s locally: make sure you're running in the folder containing `docs.json`.
