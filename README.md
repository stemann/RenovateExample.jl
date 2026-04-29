# RenovateExample.jl

Test bed for Renovate's Julia datasources, managers, and versioning
extensions.

`Project.toml` declares the public
[`Example`](https://github.com/JuliaLang/Example.jl) package as a
dependency. The pinned `[compat]` entry is intentionally older than
the current release so any working Julia-aware Renovate extension
should propose an update.

`master` carries only a minimal `renovate.json` (extending
`config:recommended`) so it can serve as the host config that
per-extension branches merge onto. The per-extension test
configurations (e.g. `customManagers` rules, `datasourceTemplate`
overrides) live on dedicated branches named after the extension
under test:

- `julia-general-metadata` — wires a `customManagers` regex rule
  that uses the
  [`julia-general-metadata`](https://github.com/renovatebot/renovate/tree/main/lib/modules/datasource/julia-general-metadata)
  datasource to look up `[compat]` entries.

To dry-run Renovate against a per-extension branch, point at it via
`baseBranchPatterns` with `useBaseBranchConfig=merge`:

```sh
RENOVATE_TOKEN=<gh-pat> \
  RENOVATE_BASE_BRANCH_PATTERNS='["julia-general-metadata"]' \
  RENOVATE_USE_BASE_BRANCH_CONFIG=merge \
  pnpm start \
    --platform=github \
    --dry-run=lookup \
    --autodiscover=false \
    stemann/RenovateExample.jl
```
