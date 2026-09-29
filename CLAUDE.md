# homebrew-tap

The Homebrew tap of Grenade: `brew install holdgrenade/tap/grenade`. Brew maps `holdgrenade/tap` to this repo, `holdgrenade/homebrew-tap`.

## Files

```
Formula/grenade.rb   the formula of the `grenade` CLI and daemon. Generated, never edited here.
README.md            what a user reads: install, update, remove
```

## How the formula changes

`Formula/grenade.rb` is a copy of `grenade-backend/packaging/homebrew/grenade.rb`, which `npm run release` writes there from `packaging/homebrew/formula.mjs`. To change the formula, change `formula.mjs`. For a new version:

1. In `grenade-backend`: `npm run release`, then publish `release/grenade-remote-<version>.tgz` as an asset of the GitHub release `v<version>`.
2. Copy `packaging/homebrew/grenade.rb` to `Formula/grenade.rb` here, commit as `grenade <version>`, push.

The `sha256` in the formula must be the one of the tarball that was uploaded: a rebuild after a source change gives another hash, so copy the formula from the same `npm run release` run as the asset.

## Checks

```bash
brew style Formula/grenade.rb
brew install holdgrenade/tap/grenade && brew test grenade   # needs the release asset to be downloadable
```

## Known limits

- The install fails until `grenade-backend` has a public release `v0.1.0` with the tarball: the asset of a private repo cannot be downloaded by brew.
