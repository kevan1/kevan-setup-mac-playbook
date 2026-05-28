# Mac App Audit - 2026-05-28

This audit reconciles `default.config.yml` against Kevan's current Mac. The
current Mac is treated as the source of truth for Homebrew formulae and casks.

## Homebrew Cask Changes

Added casks that are installed on this Mac but were missing from the playbook:

- `claude-code`
- `docker-desktop`
- `flux-markdown`
- `gcloud-cli`
- `google-chrome`
- `linearmouse`
- `middleclick`
- `mitmproxy`
- `ngrok`
- `opencode-desktop`
- `orbstack`
- `zulu@17`

Removed casks that were in the playbook but are not installed through Homebrew
on this Mac:

- `docker` replaced by `docker-desktop` for the GUI app; the `docker` formula
  remains installed for the CLI.
- `kiro`
- `transmit`
- `warp`

The Dock entry for Warp was replaced with Ghostty.

## Homebrew Formula Changes

The playbook now includes the Mac's current user-installed formulae plus the
existing installed legacy package intents. Stale package names were normalized:

- `gpg` -> `gnupg`
- `openssl` -> `openssl@3`

The following taps were added because installed formulae come from them:

- `aiken-lang/tap`
- `anomalyco/tap`
- `infisical/get-cli`
- `mobile-dev-inc/tap`
- `txpipe/tap`

## Manual App Review

These apps are present in `/Applications` but are not currently managed by the
Homebrew cask list in `default.config.yml`. Keep them manual unless a future
setup pass decides to map them to casks or other installers:

- Ami
- AmorphousDiskMark
- Android Studio
- Binky
- Claude
- Codex
- Cursor
- Dehancer Desktop
- Developer
- DockDoor
- Expo Orbit
- Grass
- Hyperkey
- Interview Buddy
- Moonlight
- NearDrop
- Nugget
- Pentagon
- PortKiller
- Quick Share
- Safari
- Sideloadly
- Stremio
- Superapp
- TinyRec
- Transmit
- Trip Way
- TypeWhisper
- Xcode
- ZoomLauncher
- checkra1n
- iDescriptor
- iRemoveTools
- minaActivatorA12
- zoom.us

`mas` is not installed on this Mac, so Mac App Store reconciliation was not
performed.
