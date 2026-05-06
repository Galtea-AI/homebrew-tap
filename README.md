# Galtea CLI — Homebrew tap

Official Homebrew tap for the [Galtea CLI](https://galtea.ai), served at <https://brew.galtea.ai>.

## Install

```bash
brew install galtea-ai/tap/galtea
```

That's it — `brew` adds the tap automatically on first install.

## Use

```bash
galtea --version
galtea login
galtea --help
```

Full command reference and tutorials at <https://docs.galtea.ai>.

## Update

```bash
brew update
brew upgrade galtea
```

## Uninstall

```bash
brew uninstall galtea
brew untap galtea-ai/tap   # optional — only if you want to drop the tap entirely
```

## How this tap works

This repository is auto-maintained — **don't edit `Formula/galtea.rb` by hand**.

Each Galtea CLI release in the [monorepo](https://github.com/Galtea-AI/monorepo) fires a `repository_dispatch` event into this repo. The [`sync.yml`](.github/workflows/sync.yml) workflow then:

1. Downloads the four `galtea_<version>_<os>_<arch>.tar.gz` archives + `checksums.txt` from the source release.
2. Mirrors the four tarballs as **assets on a GitHub Release of this tap** (tag matches the monorepo tag, e.g. [`v4.24.6`](../../releases)). The source monorepo is private; this tap is public, so its release CDN is what `brew install` actually fetches from.
3. Regenerates [`Formula/galtea.rb`](Formula/galtea.rb) with the new version + four SHA256s + URLs pointing at this tap's release-asset CDN.
4. Commits the formula update (just `Formula/galtea.rb` + `.ingested-tags` — bottles never touch git).

A daily cron also runs the same workflow as a drift catcher in case a dispatch event is ever missed.

Bottles live on GitHub Releases (not committed to git) so the repo stays tiny and `brew tap` clones in a fraction of a second forever — see [`cli/adr/0019-homebrew-tap-via-regenerate-from-tarballs.md`](https://github.com/Galtea-AI/monorepo/blob/main/cli/adr/0019-homebrew-tap-via-regenerate-from-tarballs.md) for the full design rationale.

## Other install channels

The same binary ships through every channel — pick whichever fits your setup.

| Channel | Install gesture |
|---------|-----------------|
| PyPI | `pip install galtea-cli` |
| APT (Debian/Ubuntu) | See <https://pkgs.galtea.ai> |
| YUM/DNF (Fedora/RHEL) | See <https://pkgs.galtea.ai> |
| Scoop (Windows) | `scoop install galtea` |
| Winget (Windows) | `winget install galtea` |
| Direct download | [GitHub Releases](https://github.com/Galtea-AI/monorepo/releases) |
