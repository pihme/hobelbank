# AGENTS.md

This file is for the coding agent working in this repo. Read it at the start of a session.

## What this repo is

**Hobelbank** is the map of the project family: how Hermetarium, Fregoli, grok-budget-mcp, claude-budget-mcp and the ideas without a repo yet (from the private Ideengarten) fit together. It holds docs only, no code. Main document: `README.md`; terms: `GLOSSARY.md`.

License: **PolyForm Noncommercial 1.0.0** (`LICENSE`). Source-available, not OSI Open Source. Do not relicense to Apache/MIT/GPL.

## How to work here

- Layout: `README.md` (problem, map, pieces, connections, principles, risks, related work), `GLOSSARY.md`, `docs/images/` (generated diagrams), `docs/agents/` (agent skill notes).
- Each piece's own repo is the source of truth. Only write what its code, docs, releases or open issues show; link instead of copying detail. Mark ideas and possible uses as such ("idea", "spec draft", "possible"), never as built.
- When a piece changes (release, new repo, closed issue that the map mentions), update the table and the connections here too. The relation texts must agree with `family.json` of the websites.
- No business content: no prices, revenue goals, customers or monetisation plans. This repo is meant to become public.
- Before it goes public: links to private repos (Ideengarten, presets) must be replaced or removed, and the history checked for content taken over from them.
- No tests, CI or releases; Markdown must render on GitHub (check Mermaid diagrams).

## Shared rules

### Commits and changes

- [Conventional Commits](https://www.conventionalcommits.org/) for every commit: `feat:`, `fix:`, `perf:`, `docs:`, `test:`, `refactor:`, `chore:`, `ci:`, `build:`, optional scope, `!` for breaking changes.
- **Trivial changes go straight to `main`:** typos, small docs fixes, obvious small bugs, housekeeping. Keep commits small and focused; pull before you commit.
- **Non-trivial findings become GitHub issues** (with the matching issue form): bugs you do not fix right away, design questions, anything that needs a decision. Do not hide them in a commit.
- **Never force-push**, never rewrite pushed history, never delete branches or tags.
- **No comments, reviews, label changes, closes or merges on other people's issues and PRs** unless the maintainer asked for it in the session. Exception: Dependabot PRs (see below). Opening issues for your own findings and closing them with your own commits (`Closes #n`) is fine.
- **Never write the skip-CI token** (the word `skip` and `ci` in square brackets, or any of its variants) in a commit message, not even quoted or explained: GitHub then skips CI for that push and no release is cut. Only `release.py` puts it in its own release commits.

### Secrets

- Never commit, print, log or paste tokens, API keys, credentials, `.env` files, session files or real service responses: not in code, tests, fixtures, issues, PRs or commit messages. Use synthetic fixtures.
- Security problems are reported privately (`SECURITY.md`). Do not discuss an unfixed vulnerability in a public issue.

### Contributions

- Outside pull requests are not accepted (see `CONTRIBUTING.md`); issues are.
- `CONTRIBUTING.md` holds **only content for outside people**: license, how to report, how to build and test. Internal working rules (this file) never go there.

### Website

- None yet. If one comes, the family website rules from presets apply.

## Do not invent

- No code, no scaffolding for pieces that exist only as ideas. An idea gets its own repo when the maintainer decides it.
- Do not reopen decisions recorded in the pieces' repos or in Ideengarten (`wiki/decisions.md`); describe them.

## Agent skills

### Issue tracker

GitHub issues in `pihme/hobelbank`, via `gh`. See `docs/agents/issue-tracker.md`.

### Triage labels

Default roles: `needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix`, plus `bug` / `enhancement`. See `docs/agents/triage-labels.md`.

### Domain docs

Single-context: no `SPEC.md` here; terms in root `GLOSSARY.md`. See `docs/agents/domain.md`.

## Diagrams

`docs/images/overview.svg` and `docs/images/map.svg` are generated in the same style as the family's other architecture diagrams. Change the generator and re-run it; do not edit the SVGs by hand.
