# Operational Insights REST API specs

OpenAPI 3.0 descriptions of the Couchbase Operational Insights (formerly Enterprise
Analytics) REST APIs. The published API reference on the Couchbase docs site is
generated from these files.

## Layout

One self-contained spec per API, under `openapi/`:

| File | API |
|---|---|
| `openapi/admin/admin.yaml` | Administration |
| `openapi/config/config.yaml` | Configuration |
| `openapi/iceberg/iceberg.yaml` | Iceberg |
| `openapi/link/link.yaml` | Links |
| `openapi/logging/logging.yaml` | Logging |
| `openapi/request/request.yaml` | Request (query service) |
| `openapi/settings/settings.yaml` | Cluster settings |

## Branches

Each branch describes one release line. The docs read each branch directly:

| Branch | Release | Read by docs branch |
|---|---|---|
| `helios` (default) | Operational Insights 3.0 | `release/3.0` |
| `lumina` | Enterprise Analytics 2.2 | `release/2.2` |
| `phoenix` | Enterprise Analytics 2.0, 2.1 | `release/2.0`, `release/2.1` |

`phoenix` is an ancestor of `lumina`, and `lumina` of `helios`. Make a fix on the
oldest branch it applies to, then merge it forward.

## How the docs use these files

Every API module in the docs repository has a `redocly.yaml` whose `root` is the
raw GitHub URL of one spec here, for example:

```
https://raw.githubusercontent.com/couchbase/docs-operational-insights-server/refs/heads/helios/openapi/admin/admin.yaml
```

The docs repository's `generate-docs.sh` bundles that spec with
[Redocly CLI](https://redocly.com/docs/cli/) and commits the rendered output. So:

- **A change here does not reach the docs site on its own.** Someone has to
  re-run `generate-docs.sh` on the matching docs branch and commit the result.
- **File paths are a published interface.** The docs reference them by URL, so a
  renamed or moved spec breaks the docs build. If one has to move, change the
  docs `redocly.yaml` in the same sitting.
- Operations and schemas marked `x-internal: true` are removed by the docs
  bundle (the `remove-x-internal` decorator), so use it for APIs that exist but
  should not be documented yet.

## Checking a change

```
npx @redocly/cli lint openapi/<api>/<api>.yaml
```

To preview the rendered page:

```
npx @redocly/cli build-docs openapi/<api>/<api>.yaml --output /tmp/<api>.html
```

## History

These specs were moved here from `couchbase/cbas-ui` (`docs/enterprise-analytics/spec/`,
later `docs/operational-insights/spec/`) with their full commit history.
