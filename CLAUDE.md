# homebrew-tap

The Homebrew tap of Grenade: `brew install holdgrenade/tap/grenade` for the CLI and daemon, `brew install --cask holdgrenade/tap/grenade-app` for the Mac app. Brew maps `holdgrenade/tap` to this repo, `holdgrenade/homebrew-tap`.

## Files

```
Formula/grenade.rb     the formula of the `grenade` CLI and daemon. Generated, never edited here.
Casks/grenade-app.rb   the cask of the Mac app: the release's DMG and its sha256. Generated, never edited here.
README.md              what a user reads: install, update, remove
```

## How the cask changes

`Casks/grenade-app.rb` is what `grenade-mac/scripts/cask.sh` prints for a version and its DMG's sha256; `scripts/publish.sh` there writes it to `build/release/grenade-app.rb` at each release, and that repo's release workflow commits it here as `grenade-app <version>` (author `github-actions[bot]`, through the secret `GH_HOMEBREW_TAP_TOKEN` of `grenade-mac`). To change the cask, change `cask.sh`. It is `grenade-app` because a formula and a cask cannot share the name `grenade`. The first one (1.0.169, 2026-10-09) was written by hand from the live `latest.json`, with that script.

By hand: `scripts/cask.sh <version> <sha256>` in `grenade-mac` (the sha256 is in `https://downloads.holdgrenade.com/mac/latest.json`), into `Casks/grenade-app.rb`, commit as `grenade-app <version>`, push.

## How the formula changes

`Formula/grenade.rb` is a copy of `grenade-cli/packaging/homebrew/grenade.rb`, which `npm run release` writes there from `packaging/homebrew/formula.mjs`. To change the formula, change `formula.mjs`.

A new version arrives by itself: when a push to `main` of `grenade-cli` bumps its `version`, that repo's GitHub Actions workflow (`.github/workflows/release.yml`) builds the tarball, publishes the GitHub release and npm, then commits the formula here as `grenade <version>` (author `github-actions[bot]`, through the secret `GH_HOMEBREW_TAP_TOKEN` of `grenade-cli`, a token that writes only this repo). After such a push, `git pull` here before anything else.

By hand, if the workflow is not available:

1. In `grenade-cli`: `npm run release`, then publish `release/holdgrenade-cli-<version>.tgz` as an asset of the GitHub release `v<version>`.
2. Copy `packaging/homebrew/grenade.rb` to `Formula/grenade.rb` here, commit as `grenade <version>`, push.

The `sha256` in the formula must be the one of the tarball that was uploaded: a rebuild after a source change gives another hash, so copy the formula from the same `npm run release` run as the asset.

## Checks

```bash
brew style Formula/grenade.rb Casks/grenade-app.rb
brew install holdgrenade/tap/grenade && brew test grenade   # needs the release asset to be downloadable
brew fetch --cask holdgrenade/tap/grenade-app              # downloads the DMG and checks its sha256, installs nothing
```

`brew install --cask holdgrenade/tap/grenade-app` on a Mac that runs the app from its working tree (`install-local.sh`) would replace that copy: fetch, don't install, there.

## Known limits

- The formula downloads a release asset of `grenade-cli`, so that repo has to stay public: brew cannot download the asset of a private repo.
- Installing brings Homebrew's `node`, also on a Mac that has Node from somewhere else.
