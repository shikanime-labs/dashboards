# dashboards

Grafana dashboards for the Shikanime fleet. The Grafana operator pulls each
dashboard from this repo by tag; app manifests carry only the
`GrafanaDashboard` CR pointing here.

## Layout

- `apps/<app>/dashboard.json` — one dashboard per app, raw Grafana JSON
  (importable via the Grafana UI as-is)

## Conventions

- `uid`: kebab-case, stable across edits (URLs reference it)
- `tags`: include `shikanime` plus app-specific tags
- `editable: true`, `graphTooltip: 1`
- Datasource via a `datasource` template variable, never hardcoded uid

## Workflow

1. Export or edit the JSON under `apps/<app>/dashboard.json`.
2. Open a PR; on merge, tag `vX.Y.Z` (`jj tag set vX.Y.Z -r main@origin &&
   jj git push --tag vX.Y.Z`).
3. Bump the pinned tag in the consuming
   [manifests](https://github.com/shikanime-labs/manifests) CR `spec.url`.

To preview in Grafana: Dashboard → New → Import → paste the JSON.
