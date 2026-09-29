# Grenade Homebrew tap

Homebrew formulas for [Grenade](https://www.holdgrenade.com): watch and drive AI coding agents (Claude Code, Codex, a shell) in your Mac's terminals from your phone.

## Install

```bash
brew install holdgrenade/tap/grenade
grenade setup
```

`grenade setup` checks the requirements, installs the Claude Code hooks, starts the daemon at login and pairs your phone.

## Update and remove

```bash
brew upgrade grenade
brew uninstall grenade
```

## Formulas

| Formula | What it installs |
| --- | --- |
| `grenade` | The `grenade` CLI and the `grenaded` daemon. Needs macOS; brings `node` and `tmux`. |
