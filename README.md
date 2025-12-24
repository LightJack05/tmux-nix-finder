# tmux-nix-finder

A simple tool to find shell.nix and flake.nix files in configured directories using fzf with tmux integration, and open them in tmux sessions with nix-shell or nix develop.

> [!CAUTION]
> THIS TOOL IS AI GENERATED
>
> While I have tested it, read through the code and am using it myself, I do not guarantee anything about this tool. 
> It seems to work quite well and appears like it is somewhat stable, but if it blows up, it blows up.

## Features

- **Find Nix Files**: Searches configured directories for shell.nix and flake.nix files
- **Interactive Selection**: Uses fzf with tmux integration (`fzf --tmux`) for selection
- **Session Management**: Creates a new tmux session or switches to an existing one for the selected directory
- **Smart Nix Integration**: Automatically runs `nix develop` for flake.nix or `nix-shell` for shell.nix
- **Configurable Shell**: Optionally configure which shell to run inside nix environments (e.g., zsh, bash)

## Requirements

- `bash`
- `tmux`
- `fzf`
- `nix-shell` and/or `nix develop` (part of Nix package manager)

### Installation on common systems

**NixOS / Nix package manager:**
```bash
nix-env -iA nixpkgs.tmux nixpkgs.fzf
```

**Debian/Ubuntu (with Nix installed separately):**
```bash
sudo apt install tmux fzf
# Install Nix: https://nixos.org/download.html
```

**Arch Linux (with Nix installed separately):**
```bash
sudo pacman -S tmux fzf
# Install Nix: https://nixos.org/download.html
```

**macOS (Homebrew):**
```bash
brew install tmux fzf nix
```

## Installation

1. Clone this repository or download the `tmux-nix-finder` script
2. Make it executable: `chmod +x tmux-nix-finder`
3. Move it to a directory in your PATH, e.g., `mv tmux-nix-finder ~/.local/bin/`

## Configuration

Copy the example config file to your config directory:

```bash
mkdir -p ~/.config/tmux-nix-finder
cp config.example ~/.config/tmux-nix-finder/config
```

Then edit the config file to customize your settings.

### Configuration Options

```bash
# Directories to search for shell.nix and flake.nix files
SEARCH_PATHS=("$HOME/projects" "$HOME/work")

# Maximum depth to search (default: 3)
MAX_DEPTH=3

# Optional: Command to run inside nix-shell or nix develop (e.g., "zsh", "bash")
# Leave empty for default behavior
NIX_SHELL_COMMAND="zsh"
```

## Usage

### Interactive Mode (Default)

```bash
tmux-nix-finder
```

This will:
1. Find all shell.nix and flake.nix files in configured directories
2. Present an fzf selection interface (in tmux popup)
3. On selection, create a new tmux session or switch to an existing one
4. Automatically run `nix develop` (for flake.nix) or `nix-shell` (for shell.nix) in the selected directory

### List Nix Project Directories

```bash
tmux-nix-finder --list
```

Lists all directories containing shell.nix or flake.nix files without opening them.

### Help

```bash
tmux-nix-finder --help
```

## Tmux Integration

### Bind to a tmux key

Add to your `~/.tmux.conf`:

```bash
bind-key n display-popup -E -w 80% -h 60% "tmux-nix-finder"
```

Now press `prefix + n` to launch the finder in a popup.

### Shell alias

Add to your `~/.bashrc` or `~/.zshrc`:

```bash
alias tnf='tmux-nix-finder'
```

## How It Works

1. The script searches for `shell.nix` and `flake.nix` files in the configured directories
2. When you select a directory, it creates a tmux session named after the path
3. In that session, it runs the appropriate command:
   - `nix develop` for directories with flake.nix (preferred if both files exist)
   - `nix-shell` for directories with shell.nix
4. The command sets up the development environment and drops you into a shell with all dependencies available

This is particularly useful for:
- Switching between multiple Nix development environments
- Quickly accessing project-specific tooling
- Managing isolated development shells for different projects
- Working with both legacy (shell.nix) and modern (flake.nix) Nix projects

## License

MIT License - see LICENSE file for details
