# Domain docs

How agents and skills consume this repo's domain documentation.

## Before exploring, read these

- **`README.md`**: what the workshop is and how the pieces fit. Decisions live in each piece's own repo (its `SPEC.md`) and in Ideengarten.
- **`GLOSSARY.md`** at the repo root, if it exists. If it does not, proceed silently; it is created lazily (for example by `/domain-modeling`) when terms actually get resolved.
- **`AGENTS.md`**: working rules and the "Do not invent" list.

This repo keeps no `SPEC.md` and no `docs/adr/`.

## Use the project's vocabulary

When your output names a domain concept (issue title, refactor proposal, test name), use the term as `GLOSSARY.md` defines it; do not drift to synonyms. A concept that is missing there is either invented language (reconsider) or a real gap (note it).

## Flag decision conflicts

If your output contradicts a decision in a piece's `SPEC.md`, say so explicitly instead of silently overriding:

> _Contradicts the decision on X, but worth reopening because…_
