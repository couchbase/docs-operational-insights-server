# CLAUDE.md

This repository holds the OpenAPI specs for the Operational Insights REST APIs, one
file per API under `openapi/`. There is no code and no build. `README.md` covers
the layout, the branch-to-release mapping, and how the docs consume these files.
Read it first.

## Rules

- **Do not rename or move a spec file.** The docs repository fetches each one by
  raw GitHub URL on a specific branch, so a path change silently breaks the docs
  build for that release. If a move is truly needed, say so and stop. It needs a
  coordinated change to the docs `redocly.yaml` files.
- **Keep each spec self-contained.** Only use `$ref`s to `#/components/...` in
  the same file. A relative `$ref` to another file would be resolved against the
  raw URL at docs build time, which is fragile.
- **Pick the branch by release, not by convenience.** `helios` is Operational
  Insights 3.0, `lumina` is Enterprise Analytics 2.2, and `phoenix` is
  Enterprise Analytics 2.0/2.1. Product naming differs by branch: specs on
  `helios` say "Operational Insights" and specs on `phoenix`/`lumina` say
  "Enterprise Analytics". Do not carry naming across.
- **Fix on the oldest affected branch, then merge forward**
  (`phoenix` → `lumina` → `helios`). Do not cherry-pick backwards.
- **Describe the server as it behaves.** A spec is the contract readers code
  against. When documenting a parameter, response or status code, check it
  against the server implementation rather than inferring it from neighbouring
  endpoints. Hide APIs that are not ready for publication with `x-internal: true`
  rather than leaving them out.
- Stay on OpenAPI `3.0.3`, which all the specs use.

## Checking a change

Lint every spec you touch before committing:

```
npx @redocly/cli lint openapi/<api>/<api>.yaml
```

Lint passing doesn't prove the content is accurate. It only shows the file is
well-formed.

## Commit messages

- Subject: `<TICKET>: <imperative summary>`, at most 50 characters, for example
  `MB-74367: Drop the Local link from the Links API spec`.
- Keep the body short: what changed in the API description, and why.
- End with the attribution trailer, naming the model that did the work:

  ```
  Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>
  ```
