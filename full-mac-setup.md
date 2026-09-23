# Full Mac Setup Process (for Kevin Anrique)

There are some things in life that just can't be automated... or aren't 100% worth the time :(

This document covers that, at least in terms of setting up a brand new Mac out of the box.

## Important: SSH Keys and Secrets

**SSH private keys are NOT in the dotfiles repository.** After reinstalling macOS:

1. Restore SSH keys from local backup: `~/Documents/kevan-reinstall-backup/ssh/`
2. Set correct permissions:
   ```bash
   chmod 700 ~/.ssh
   chmod 600 ~/.ssh/id_*
   chmod 644 ~/.ssh/*.pub
   ```
3. **Never commit SSH private keys or API tokens to GitHub**
4. Manually restore secrets to local environment files (e.g., `~/.config/walter-os/overlay/personal.env`)

## Terminal: Ghostty

This setup uses **Ghostty** as the terminal emulator (not Warp). Install via Homebrew:

```bash
brew install --cask ghostty
```

The Ghostty config was empty at collection time. Customize `~/Library/Application Support/com.mitchellh.ghostty/config.ghostty` after installation.

## Login Items

After setup, configure these apps to launch at login (System Settings → General → Login Items):

- **Raycast** - Install via `brew install --cask raycast`, then sign in to sync settings
- **Bitwarden** - Install via `brew install --cask bitwarden`
- **Cloudflare WARP** (optional) - Not in playbook casks; install manually if needed

See `dotfiles/docs/login-items.md` for details.

## Initial configuration of a brand new Mac

Before starting, I completed Apple's mandatory macOS setup wizard (creating a local user account, and optionally signing into my iCloud account). Once on the macOS desktop, I do the following (in order):

  - Install Ansible (following the guide in [README.md](README.md))
  - **Sign in to App Store** (since `mas` can't sign in automatically)
    - Optionally install App Store apps like Xcode, WhatsApp, Apple Configurator, etc.
    - `mas` CLI can be used in the future, but for now install these manually
  - Clone kevan-setup-mac-playbook to the Mac: `git clone git@github.com:kevan1/kevan-setup-mac-playbook.git`
  - Create `config.yml` if you want to override any defaults (optional - see README)
  - Run the playbook: `ansible-playbook main.yml --ask-become-pass`
    - If there are errors, troubleshoot and run again
  - Start Synchronization tasks:
    - Open Photos and make sure iCloud sync options are correct
    - Sign into Bitwarden and sync

## Container/Docker Environment

  - **OrbStack** is installed via Homebrew cask instead of Docker Desktop
  - OrbStack provides Docker CLI, docker-compose, and buildx functionality
  - No need to install separate `docker`, `docker-buildx`, or `docker-compose` formulae
  - After first launch, configure OrbStack preferences as needed

## VPN and Networking

  - **Tailscale** is installed via Homebrew formula (not the cask)
    - Launch Tailscale and sign in with your account
  - If you need **Cloudflare WARP**, install it manually (not part of automated playbook)
  - `cloudflared` CLI is installed via Homebrew for tunnel management

## Manual App Installations

The following apps need to be installed manually (not available or not automated):

### Development Tools
  - **Cursor** - Primary code editor (install manually from https://cursor.sh)
  - **ChatGPT** - Desktop app (install manually; required for Dock entry to work)
  - **Claude** - Anthropic's desktop app (install manually if desired)
  - **Grok Bot** - Install manually if desired
  - **Sideloadly** - iOS sideloading tool (install manually if needed)
  - **Mole** - Documented as desired (brew formula `mole` is installed but check for GUI app)

### Explicitly NOT Part of Playbook

The following were previously in the playbook or exist on your Mac but are intentionally excluded:

  - ❌ **Surfshark** - Removed from casks
  - ❌ **Kiro** - Removed from casks  
  - ❌ **Google Drive** - Removed from casks
  - ❌ **Docker Desktop** - Replaced by OrbStack
  - ❌ **Warp** - Removed from casks (using Ghostty instead)
  - ❌ **Visual Studio Code** - Removed from casks (using Cursor instead)
  - ❌ **Arc Browser** - Removed from casks
  - ❌ **CleanMyMac** - Removed from casks
  - ❌ **DaisyDisk** - Removed from casks
  - ❌ **Middle** - Removed from casks
  - ❌ **Google Chrome** - Not automated (install manually if needed, but not in playbook)

## SSH Key Setup

**IMPORTANT**: SSH private keys are NOT in the dotfiles repository!

After reinstalling macOS, restore from local backup:

```bash
# Restore from backup: ~/Documents/kevan-reinstall-backup/ssh/
# DO NOT symlink to cloud storage or commit to GitHub
cp -r ~/Documents/kevan-reinstall-backup/ssh/* ~/.ssh/
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_*
chmod 644 ~/.ssh/*.pub
```

Alternative: Generate new SSH keys or restore from Bitwarden
  - Example: `ssh-keygen -t ed25519 -C "kevan@kevan.com.ar"`
  - Store private keys securely in Bitwarden
  - Add public keys to GitHub, GitLab, servers, etc.
  - Configure `~/.ssh/config` for your hosts (do not commit sensitive paths)

## Development Environment Setup

The playbook installs many modern development tools via Homebrew:

### Version Managers
  - **mise** - Modern runtime version manager (replaces nvm/asdf)
  - **uv** - Fast Python package installer

### Shell Enhancements
  - **bash** - Modern Bash shell
  - **zoxide** - Smarter cd command

### Developer Tools
  - **ansible** - Automation tool
  - **gh** - GitHub CLI
  - **glab** - GitLab CLI
  - **gitleaks** - Secret scanning
  - **shellcheck** - Shell script linting
  - **bats-core** - Bash testing framework

### Platform-Specific Tools
  - **cocoapods** - iOS dependency manager
  - **eas-cli** - Expo Application Services (npm global)
  - **solana** - Solana blockchain tools
  - **typst** - Modern typesetting system

### Cloud & Infrastructure
  - **hcloud** - Hetzner Cloud CLI
  - **cloudflared** - Cloudflare tunnel client
  - **infisical** - Secret management
  - **supabase** - Supabase CLI

### Testing & Mobile Development
  - **maestro** - Mobile UI testing framework
  - **skills** - npm global package

### Other Utilities
  - **ddrescue** - Data recovery tool
  - **exiftool** - Image metadata tool
  - **flock** - File locking utility
  - **ykman** - YubiKey manager
  - **yq** - YAML/XML/TOML processor
  - **coreutils** - GNU core utilities

## Dock Configuration

The playbook automatically configures your Dock to mirror your current Mac setup:

1. Dia
2. Messages
3. Mail
4. Calendar
5. Ghostty (terminal)
6. ChatGPT (must install manually first)
7. Spotify
8. System Settings

Downloads folder is configured in the Dock as a stack.

**Note**: ChatGPT must be installed manually before running the playbook for the Dock entry to work properly.

## Terminal and Dotfiles

The playbook is configured to use your personal dotfiles repository:

  - **Dotfiles repo**: `https://github.com/kevan1/dotfiles`
  - **Branch**: `main`
  - Files synced: `.zshrc`, `.zprofile`, `.zshenv`, `.gitconfig`, `.gitignore`, `.inputrc`, `.osx`, `.vimrc`
  - The dotfiles repo contains sanitized configs (secrets redacted)
  - See `dotfiles/docs/` for setup notes on Ghostty, Raycast, login items, and macOS defaults
  - Secrets must be manually restored to local environment files (e.g., `~/.config/walter-os/overlay/personal.env`)

## Things That Can't Be Automated

  - **App Store sign-in** - Must be done manually
  - **iCloud sync** - Configure in System Settings
  - **Bitwarden login** - Sign in and configure vault
  - **Tailscale authentication** - Sign in after installation
  - **Time Machine** - Configure backup drive manually
  - **Browser profiles** - Sign into Chrome/Dia/Safari
  - **Transmit sync** - Configure Panic Sync if needed
  - **Raycast** - Configure shortcuts and extensions after first launch
  - **Font installation** - Custom fonts in ~/Library/Fonts (manual)

## Post-Installation Manual Steps

  - Manual system preferences to configure:
    - Accessibility > Display > Reduce transparency
    - Keyboard > Keyboard Shortcuts... > Modifier Keys... > Caps Lock to Esc
    - Keyboard > Key repeat rate to 'Fast', Delay until repeat to 'Short'
    - Privacy & Security > Full Disk Access > enable "Ghostty" or your terminal
  - Finder settings:
    - Disable click-to-show Desktop: `defaults write com.apple.WindowManager EnableStandardClickToShowDesktop -bool false`
  - Configure any VPN connections (Tailscale, Wireguard, etc.)

## When Formatting Old Mac

Before wiping your old Mac:

  - Sign out of important apps (App Store, Bitwarden, etc.)
  - Make sure Bitwarden vault is synced with latest SSH keys
  - Deauthorize Apple Music
  - Export any custom configurations not in this playbook
  - Back up any local-only data
  - Follow Apple's guide [here](https://support.apple.com/en-au/HT212749)
