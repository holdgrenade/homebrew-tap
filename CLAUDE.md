# homebrew-tap

The Homebrew tap of Grenade: `brew install holdgrenade/tap/grenade`. Brew maps `holdgrenade/tap` to this repo, `holdgrenade/homebrew-tap`.

## Files

```
Formula/grenade.rb   the formula of the `grenade` CLI and daemon. Generated, never edited here.
README.md            what a user reads: install, update, remove
```

## How the formula changes

`Formula/grenade.rb` is a copy of `grenade-cli/packaging/homebrew/grenade.rb`, which `npm run release` writes there from `packaging/homebrew/formula.mjs`. To change the formula, change `formula.mjs`.

A new version arrives by itself: when a push to `main` of `grenade-cli` bumps its `version`, that repo's GitHub Actions workflow (`.github/workflows/release.yml`) builds the tarball, publishes the GitHub release and npm, then commits the formula here as `grenade <version>` (author `github-actions[bot]`, through the secret `RELEASE_TOKEN` of `grenade-cli`). After such a push, `git pull` here before anything else.

By hand, if the workflow is not available:

1. In `grenade-cli`: `npm run release`, then publish `release/holdgrenade-cli-<version>.tgz` as an asset of the GitHub release `v<version>`.
2. Copy `packaging/homebrew/grenade.rb` to `Formula/grenade.rb` here, commit as `grenade <version>`, push.

The `sha256` in the formula must be the one of the tarball that was uploaded: a rebuild after a source change gives another hash, so copy the formula from the same `npm run release` run as the asset.

## Checks

```bash
brew style Formula/grenade.rb
brew install holdgrenade/tap/grenade && brew test grenade   # needs the release asset to be downloadable
```

## Known limits

- The formula downloads a release asset of `grenade-cli`, so that repo has to stay public: brew cannot download the asset of a private repo.
- Installing brings Homebrew's `node`, also on a Mac that has Node from somewhere else.
