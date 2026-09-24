# Dashboards

Grafana dashboards for the Shikanime fleet, sourced by the Grafana operator
from this repository.

**Language:** JSON (Grafana dashboard model)

## Structure

- `dashboards/<name>.json` — one dashboard per topic
- `flake.nix` — devlib devenv shell

## Layout

Single dashboard per topic at `dashboards/<name>.json`; nest under
`dashboards/<topic>/` only when a topic needs multiple dashboards. Files are
raw Grafana JSON, importable via the Grafana UI as-is.

## Conventions

- `uid`: kebab-case, stable across edits (URLs reference it)
- `tags`: include `shikanime` plus topic-specific tags
- `editable: true`, `graphTooltip: 1`
- Datasource via a `datasource` template variable, never a hardcoded uid
- Consumers pin a release tag in the `GrafanaDashboard` CR `spec.url`; bump the
  tag in [manifests](https://github.com/shikanime-labs/manifests) on release

## Commit Style

- Plain-text capitalized title, no conventional-commit prefix
- Body with labels: `Design:`, `Related:`, `Closes #`
- Keep Markdown lines wrapped at 80 columns and run `nix fmt` before shipping

## Stack Workflow

- Install the official GitHub extension once: `gh extension install github/gh-stack`
  (requires GitHub CLI ≥ 2.0; `gh stack` is in public preview and may change).
- Keep one logical change per PR; split large work into a stack of PRs.
- Create a stack: `gh stack init`, then `gh stack add` for each new branch, and
  commit on the active branch. `gh stack view` lists the stack.
- Submit/update: `gh stack submit` (add `--open` to open PRs, `--auto` to skip
  prompts). Resubmit after each change to refresh titles, bodies, and branches.
- Pull down an existing stack: `gh stack checkout <PR_NUMBER>` (also accepts a
  stack number, PR URL, or branch name).
- Rebase onto updated trunk: `gh stack rebase` (cascading), then `gh stack submit`.
- Land a stack: `gh stack merge` (interactive) or
  `gh stack merge <PR_NUMBER> --yes --squash` to merge up to a PR.
- Never `gh pr merge` on a stacked PR — only `gh stack merge` lands stacks.
- Never force-push stack branches; `gh stack` owns the branch pointers.

## Protect `main`

- Require 1 approving review
- Require linear history (no merge commits)
- Require signed commits
- Squash+rebase merge only

## Environment

This repository ships a `.envrc` for direnv. Run `direnv allow` once after
cloning; direnv then loads the Nix flake dev shell automatically on every
directory change (`.envrc` runs
`use flake . --accept-flake-config --no-pure-eval`). Without direnv, enter
the same shell manually with `nix develop`.
