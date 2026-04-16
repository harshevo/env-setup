# setup-env

One-shot dev environment bootstrap for macOS, Debian/Ubuntu, and Arch.

## Usage

```sh
./setup            # install missing packages and configs (idempotent)
./setup --update   # also upgrade installed packages and rewrite tracked configs
./setup --help
```

The script is safe to re-run: anything already present is skipped unless `--update` is passed.

## What it installs

**CLI tools**
- `fzf`, `fd`, `ripgrep`, `tmux`, `neovim`, `git`
- `nvm` + Node.js LTS

**Language toolchains**
- Python 3 (+ pip / venv on Debian)
- C/C++: `gcc`, `clang`, `build-essential` / `base-devel`
- Go

**Desktop apps**
- VS Code, DBeaver CE, Google Cloud SDK, Docker, Telegram
  (via Homebrew casks on macOS; vendor apt repos on Debian; pacman/AUR notes on Arch)
- Adds your user to the `docker` group on Linux

## What it configures

- `~/.tmux.conf` from this repo + TPM (tmux plugin manager)
- `~/.local/scripts/tmux-dev` launcher script
- Shell rc (`.zshrc` / `.bashrc`) blocks:
  - PATH export for `~/.local/bin` and `~/.local/scripts`
  - `sd()` fzf directory jumper
  - Ctrl+F zsh widget that runs `tmux-dev .`
- Terminal Ctrl+F bindings: iTerm2 (plist), Alacritty (TOML); Terminal.app note
- Clones / pulls [Nvim-evo](https://github.com/harshevo/Nvim-evo) into `~/.config/nvim`

Existing files are backed up as `<file>.bak.<timestamp>` before being overwritten. Symlinks are resolved first so the real target is what gets backed up.

## `--update` behavior

- `brew update` / `pacman -Syu` / `apt-get update`, then upgrade each tracked package
- `nvm install --lts --reinstall-packages-from=current`
- Rewrites shell rc blocks and Alacritty / iTerm2 bindings in place
- `git pull` the Nvim-evo repo
