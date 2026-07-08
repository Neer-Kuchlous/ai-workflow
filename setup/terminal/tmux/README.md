# tmux

tmux is the persistent terminal workspace.
Use it for sessions, windows, scrollback, and long-lived project work.

## Install

Install tmux with Homebrew:

```bash
brew install tmux
```

Verify the install:

```bash
tmux -V
```

## Baseline Config

Edit `~/.tmux.conf`:

```bash
nano ~/.tmux.conf
```

Use these baseline settings:

```tmux
set -g history-limit 50000
set -g set-titles on
set -g set-titles-string '#S'
```

The `set-titles-string '#S'` setting makes the native terminal tab title follow the active tmux session name dynamically.

Apply changes to the running tmux server:

```bash
tmux source-file ~/.tmux.conf
```

## Personal Status Bar Styling

Personal status bar styling is optional.
Neer's current styling is documented in [personal preferences](../../personal-preferences/README.md).

## Basic Commands

Create a named session:

```bash
tmux new -s ai-workflow
```

Attach to an existing session:

```bash
tmux attach -t ai-workflow
```

List sessions:

```bash
tmux list-sessions
```

List windows in a session:

```bash
tmux list-windows -t ai-workflow
```

Rename the current session:

```bash
tmux rename-session ai-workflow
```

Inside tmux, show the current session name:

```bash
tmux display-message -p '#S'
```
