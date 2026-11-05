# RenovateExample.jl

Test bed for Renovate's Julia support.

`master` is for running Renovate on the repository as it is, with the Renovate GitHub app or self-hosted, on default config (`config:recommended`).
Its `Project.toml` and `docs/Project.toml` have outdated `[compat]` entries, each showing how Renovate's `julia` manager handles one kind of entry:

| Entry | Renovate proposes |
|---|---|
| `Compat = "3"` | `"3, 4"`, a major update |
| `OrderedCollections = "1"` | `"1, 2"`, a major update |
| `DocStringExtensions = "0.8"` | `"0.8, 0.10"` |
| `PrecompileTools = "~1.0"` | `"~1.0, ~1.3"` |
| `Preferences = "=1.4.0"` | `"=1.6.1"`: an exact version stays exact |
| `Example = "0.4.0"` | `"0.4.0, 0.5"` |
| `Aqua = "0.6"`, a test dependency in `[extras]` and `[targets]` | `"0.6, 0.8"`, with the `depType` `test` |
| `StaticArrays = "0.12"`, in `[weakdeps]` | `"0.12, 1.9"`, with the `depType` `weakdeps` |
| `Documenter = "0.27"`, in `docs/Project.toml` | `"0.27, 1.19"` |
| `ScopedValues = "1"` | nothing: it allows the latest release |
| `LinearAlgebra`, `Test` and `julia` | nothing: the manager skips them |
| `RenovateExample`, from `[sources]` in `docs/Project.toml` | nothing: it comes from a local path |

The Renovate pull requests on `master` are this showcase, so they are left open.
Renovate opens two an hour, and the Dependency Dashboard lists the rest until then.

The other branches each test one part of Renovate's Julia support:

| Branch | Tests | Pull request |
|---|---|---|
| [`julia-general-metadata`](https://github.com/stemann/RenovateExample.jl/tree/julia-general-metadata) | the `julia-general-metadata` datasource, through a `customManagers` regex | renovatebot/renovate#42994 |
| [`julia-versioning`](https://github.com/stemann/RenovateExample.jl/tree/julia-versioning) | `julia` versioning, through a `customManagers` regex | renovatebot/renovate#42995 |
| [`julia-manager`](https://github.com/stemann/RenovateExample.jl/tree/julia-manager) | the `julia` manager | renovatebot/renovate#42996 |
| [`julia-datasource`](https://github.com/stemann/RenovateExample.jl/tree/julia-datasource) | the `julia` datasource, with General and HolyLabRegistry | renovatebot/renovate#47699 |
| [`julia-manifest`](https://github.com/stemann/RenovateExample.jl/tree/julia-manifest) | updating `Manifest.toml`, written by Julia 1.10.12 | renovatebot/renovate#47788 |

To dry-run Renovate on a branch, point at it with `baseBranchPatterns` and `useBaseBranchConfig=merge`:

```sh
RENOVATE_TOKEN=<gh-pat> \
  RENOVATE_BASE_BRANCH_PATTERNS='["julia-manager"]' \
  RENOVATE_USE_BASE_BRANCH_CONFIG=merge \
  pnpm start \
    --platform=github \
    --dry-run=lookup \
    --autodiscover=false \
    stemann/RenovateExample.jl
```

`julia-manifest` needs `--dry-run=full`, which runs Pkg, and `RENOVATE_BINARY_SOURCE=global` with Julia 1.10.12 or juliaup on the `PATH`.
