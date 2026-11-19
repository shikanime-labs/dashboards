<!-- owner: Automata | zone: technical | purpose: Canonical technical index for the Grafana dashboards repository -->
# Dashboards

Grafana dashboards for the Shikanime fleet. Each dashboard is a raw Grafana JSON
model stored at `dashboards/<name>.json`; app manifests carry only a
`GrafanaDashboard` CR pointing here, and the Grafana operator pulls the model by
tag. JSON is the only language in this repo.

## Catalog

| File | Title | uid |
| --- | --- | --- |
| `blackbox.json` | Blackbox | `blackbox` |
| `blocky.json` | Blocky | `blocky-dns` |
| `comin.json` | Comin | `comin` |
| `halogen-flash.json` | Halogen Flash | `halogen-flash-monitoring` |
| `honcho.json` | Honcho | `honcho` |
| `inference.json` | Inference | `inference` |
| `llama-cpp.json` | llama.cpp | `llamacpp-monitoring` |
| `power.json` | Power Usage | `power` |
| `security.json` | Security | `security` |

A topic needing more than one dashboard nests under `dashboards/<topic>/`.

## JSON conventions

- `uid`: kebab-case and stable across edits, because Grafana URLs reference it.
- `tags`: include `shikanime` plus topic-specific tags.
- `editable: true` and `graphTooltip: 1`.
- Datasource is set through a `datasource` template variable, never a
  hardcoded uid.

## Release workflow

1. Edit the JSON under `dashboards/<name>.json`.
2. Open a PR. On merge, tag `vX.Y.Z`.
3. Bump the pinned tag in the consuming `GrafanaDashboard` CR `spec.url` in
   [manifests](https://github.com/shikanime-labs/manifests); the operator pulls
   by tag.

Preview locally with Grafana &rarr; New &rarr; Import &rarr; paste the JSON.

## Tooling and CI

A devlib-based devenv shell is defined in `flake.nix`. GitHub Actions cover
cleanup, integration, nix, release, and triage workflows under
`.github/workflows/`.

## Repository layout

- `dashboards/` &mdash; one JSON model per topic (the catalog above)
- `flake.nix` / `flake.lock` &mdash; devenv shell and pinned inputs
- `.github/workflows/` &mdash; cleanup, integration, nix, release, triage
- `AGENTS.md` &mdash; agent-facing conventions
- `docs/` &mdash; this canonical index
- `README.md` &mdash; human-facing overview

## Canonical sources

- [Repository README](https://github.com/shikanime-labs/dashboards#readme)
- [Technical wiki](https://github.com/shikanime-labs/dashboards/wiki)
  ([Catalog](https://github.com/shikanime-labs/dashboards/wiki/Catalog),
  [Conventions](https://github.com/shikanime-labs/dashboards/wiki/Conventions))

In-repo `docs/` is canonical for technical detail; the wiki is a rendered human
index that links here.
