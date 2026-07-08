# Personal Preferences

This document tracks personal setup preferences for Neer's development environment.
Use this file for personalization, not required project functionality.
Anything that another developer or AI agent must have for the workflow to function should go in the relevant setup module instead.

Examples of personal setup:

- terminal appearance
- font choices
- color themes
- keyboard preference tweaks
- tmux status bar styling
- shell prompt preferences
- editor preference notes
- personal aliases
- personal project startup shortcuts

Examples that do not belong here:

- required install steps
- required project commands
- required environment variables
- required CI or deploy setup
- team-wide agent instructions

## tmux Pastel Blue Status Bar

Added personal tmux status bar styling in `~/.tmux.conf`.

Purpose:

- change the bottom tmux status bar from green to pastel light blue
- make the active window slightly darker than the rest of the status bar
- keep status text readable with dark foreground colors

Settings added:

```tmux
set -g status-style 'bg=#b8dff5,fg=#26323a'
set -g window-status-style 'bg=#b8dff5,fg=#4f5f68'
set -g window-status-current-style 'bg=#8fc8eb,fg=#17242d,bold'
```

To revert this styling, remove those lines from `~/.tmux.conf` and run:

```bash
tmux source-file ~/.tmux.conf
```

## WezTerm Beige Terminal Palette

Added a personal beige terminal palette in `~/.config/wezterm/wezterm.lua`.

Purpose:

- change the default terminal background from black to beige
- use a darker foreground so normal text remains readable
- define matching cursor, selection, ANSI, and bright ANSI colors
- keep Codex/TUI regions readable by using a lighter ANSI black value
- use light gray for dark-gray TUI regions such as the Codex composer when the app uses indexed terminal colors
- use pastel red and pastel green for deleted and added diff regions
- use a darker bright green so Codex quoted text remains readable on beige backgrounds

Settings added inside `config.colors`:

```lua
foreground = '#2c261c',
background = '#f1e4c9',
cursor_bg = '#6f5632',
cursor_fg = '#f1e4c9',
cursor_border = '#6f5632',
selection_bg = '#d7c49d',
selection_fg = '#2c261c',
ansi = {
  '#d8c7a8',
  '#f4b8b8',
  '#bfe8c4',
  '#8a6626',
  '#365f8f',
  '#76518f',
  '#3f7370',
  '#eadbbd',
},
brights = {
  '#e5e7eb',
  '#ffd1d1',
  '#2f7d4f',
  '#a77d32',
  '#4f77a8',
  '#8c68aa',
  '#57908c',
  '#fff6df',
},
indexed = {
  [232] = '#e5e7eb',
  [233] = '#e5e7eb',
  [234] = '#e5e7eb',
  [235] = '#e5e7eb',
  [236] = '#e5e7eb',
  [237] = '#e5e7eb',
  [238] = '#e5e7eb',
},
```

To revert only this palette, remove those settings from `config.colors`.

## WezTerm Tab Bar Styling

Added personal WezTerm tab bar styling in `~/.config/wezterm/wezterm.lua`.

Purpose:

- keep the top WezTerm tab bar visible
- replace the default fancy tab style with a simpler beige tab bar
- make the active tab more visually distinct
- make the top tab bar taller and closer to a browser-tab size
- add horizontal padding so tab labels feel wider

To revert this styling, remove the `tab_title` helper, the `format-tab-title` event handler, `config.window_frame`, and the tab-related settings from `~/.config/wezterm/wezterm.lua`.
