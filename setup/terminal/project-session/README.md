# Project Session Workflow

Use a terminal-first workflow built around WezTerm, tmux, and Codex.
WezTerm owns the macOS terminal window.
tmux owns sessions, windows, scrollback, and long-lived project workspaces.
Codex runs from the repository root inside tmux.

## Start A Project Session

Create or attach to a project session:

```bash
tmux new -s ai-workflow
```

Start Codex from the project folder:

```bash
cd ~/Code/ai-workflow
codex
```

## tmux Windows

Use tmux windows for separate long-lived activities:

- Codex
- shell commands
- dev servers
- logs
- tests

List windows:

```bash
tmux list-windows
```

Create a new window:

```bash
tmux new-window
```

Rename the current window:

```bash
tmux rename-window shell
```

## Scrollback

tmux owns useful scrollback.
Set a large history limit in `~/.tmux.conf`:

```tmux
set -g history-limit 50000
```

Reload the config:

```bash
tmux source-file ~/.tmux.conf
```

## Editing Files

Use the editor that fits the task.
Neovim is a good terminal-native option:

```bash
nvim path/to/file
```

Codex can also edit files directly when the requested change is clear.

## Codex Composer Editing

Use normal shell and terminal editing shortcuts in the Codex composer.
Keep prompts short enough to review before sending.
For larger instructions, put the details in a file and reference that file from the prompt.
