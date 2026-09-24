# Dashboards

Grafana dashboards for the Shikanime fleet. The Grafana operator pulls each
dashboard from this repo by tag; app manifests carry only the
`GrafanaDashboard` CR pointing here.

## Layout

- `dashboards/<name>.json` — one dashboard per topic, raw Grafana JSON
  (importable via the Grafana UI as-is); nest under `dashboards/<topic>/`
  only when a topic needs multiple dashboards

## Conventions

- `uid`: kebab-case, stable across edits (URLs reference it)
- `tags`: include `shikanime` plus topic-specific tags
- `editable: true`, `graphTooltip: 1`
- Datasource via a `datasource` template variable, never a hardcoded uid

## Workflow

1. Edit the JSON under `dashboards/<name>.json`.
2. Open a PR; on merge, tag `vX.Y.Z` and bump the pinned tag in the consuming
   [manifests](https://github.com/shikanime-labs/manifests) CR `spec.url`.

To preview in Grafana: Dashboard → New → Import → paste the JSON.
