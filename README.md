# spinel-index

The [spinelgems](https://github.com/matz/spinel) index: a git-hosted map
from gem names to their repositories and releases. There is no registry
server; `spin` clones this repo into its cache and resolves
`[dependencies] name = "constraint"` entries against it.

## Format

One TOML file per gem, `gems/<name>.toml`:

```toml
name = "ansi"
repo = "https://github.com/you/spinel-ansi"

[[release]]
version = "1.0.0"
ref = "a94a8fe5ccb19ba61c4c0873d391e987982fbbd3"  # full commit SHA in `repo`
```

- `version` follows `x.y.z`; constraints in gem.toml are `"~> x.y"`,
  `">= x.y.z"`, exact, or `"*"`.
- Selection is MVS: `spin` picks the **lowest** release satisfying every
  constraint, and `gem.lock` pins the outcome.
- Same name as a rubygems.org gem means "the same library, possibly a
  subset-compatible port"; diverging forks must rename (`foo-spinel`).

## Adding a gem

Open a pull request adding or updating `gems/<name>.toml`. Each release's
`ref` must be a full commit SHA reachable in `repo` whose tree contains a
`gem.toml` with the matching `version`.
