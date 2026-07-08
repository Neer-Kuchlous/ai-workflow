# WezTerm

WezTerm is the terminal emulator for the terminal-first AI workflow.
tmux owns sessions and windows inside WezTerm.
Codex runs inside tmux from the project folder.

## Install

Install WezTerm with Homebrew:

```bash
brew install --cask wezterm
```

Open WezTerm:

```bash
open -a WezTerm
```

Verify the shell:

```bash
echo "$SHELL"
echo "$0"
```

The expected shell is `zsh`.

## Baseline Config

Create the config directory and file:

```bash
mkdir -p ~/.config/wezterm
nano ~/.config/wezterm/wezterm.lua
```

Use this baseline config:

```lua
local wezterm = require 'wezterm'

local config = wezterm.config_builder()

config.default_prog = { '/bin/zsh', '-l' }

config.font = wezterm.font_with_fallback {
  'JetBrains Mono',
  'Menlo',
  'Monaco',
}

config.font_size = 14
config.initial_cols = 120
config.initial_rows = 32
config.scrollback_lines = 10000

config.hide_tab_bar_if_only_one_tab = true

return config
```

Quit and reopen WezTerm after changing this file.

## Tab Titles

The recommended tmux config publishes the current tmux session name as the terminal title.
The existing WezTerm config can use `tab.active_pane.title` to show that title in the native tab bar.

If the WezTerm tab title does not update immediately, switch tabs once or restart WezTerm.
