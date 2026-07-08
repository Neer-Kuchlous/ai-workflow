# FirstMate

FirstMate turns one terminal coding agent into an orchestrator for a visible crew of agents.
You talk to the first mate from one Codex session, and FirstMate spawns supervised crewmates in isolated worktrees and terminal windows.
Use it for parallel fixes, investigations, audits, plans, pull requests, and local approved merges.

FirstMate is not a normal app, npm package, or single CLI.
It is a Git repository whose `AGENTS.md`, bundled skills, and helper scripts teach the running agent how to operate the crew.

## Prerequisites

- Codex is installed and logged in.
- tmux is installed.
- GitHub CLI is installed and authenticated.
- Node and npm are installed.
- Git remotes use HTTPS and GitHub CLI manages credentials.
- `gh-axi` and `lavish-axi` are available through `npx`.

Install the basic prerequisites:

```bash
brew install tmux gh node
gh auth login --web --git-protocol https
gh auth setup-git
codex login status
```

## Install

Clone FirstMate into the normal code workspace:

```bash
cd "$HOME/Code"
git clone https://github.com/kunchenguid/firstmate.git
cd "$HOME/Code/firstmate"
```

## Launch

Launch FirstMate from inside tmux:

```bash
tmux new -s firstmate
```

Inside that tmux session, start Codex from the FirstMate repo root:

```bash
cd "$HOME/Code/firstmate"
codex
```

When Codex asks whether to trust the directory, choose yes.
FirstMate needs the repository instructions and local files to load so `AGENTS.md` can take over.

Start with a small prompt:

```text
hello
```

Expected behavior:

- FirstMate runs its session-start digest.
- It acquires the fleet lock.
- It reports current projects, backlog, wake queue, and in-flight tasks.
- It does not say the session is read-only.

## Fleet Lock Troubleshooting

If FirstMate says it is read-only because it could not acquire the fleet lock, check from the shell:

```bash
cd "$HOME/Code/firstmate"
bin/fm-lock.sh status
bin/fm-harness.sh
```

Before Codex starts, `bin/fm-harness.sh` may print `unknown`.
That is normal from a plain shell because no agent harness is above the command yet.
The real test is whether the command works when FirstMate runs it from inside Codex.

If the shell output includes this macOS login-shell error:

```text
basename: illegal option -- z
```

then the local FirstMate checkout has the macOS `-zsh` process-name bug.
Update FirstMate first:

```bash
cd "$HOME/Code/firstmate"
git pull --ff-only
```

If the bug still exists, patch the local checkout so process-name detection calls `basename -- "$comm"` instead of `basename "$comm"` in:

```text
bin/fm-harness.sh
bin/fm-lock.sh
```

After patching, restart the FirstMate Codex session.

## Codex Skill Mentions

Use Codex skill mentions for FirstMate's built-in skills.
Claude and Grok examples may show slash commands, but Codex uses `$` skill mentions.

Examples:

```text
$afk
$bearings
$updatefirstmate
$stow
```

## Watching Crew Windows

Watch crew windows from tmux when tasks are running:

```bash
tmux list-windows -t firstmate
tmux select-window -t firstmate:fm-<id>
```

Do not paste split-pane divider characters such as `│` into shell commands.
That character can create accidental paths such as `~/│` or become an extra command argument.
If an accidental duplicate FirstMate clone appears under `~/│`, verify it is a duplicate before deleting it.

Keep FirstMate's project edits behind the crew workflow.
The first mate should orchestrate, supervise, review, and merge only with explicit approval.
Crewmates should do project implementation work in isolated task worktrees.
