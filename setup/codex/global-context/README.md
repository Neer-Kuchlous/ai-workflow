# Global Codex Context

Use global Codex context files for instructions that should apply across all projects on a machine.
Keep durable personal opinions and communication voice in separate reference files so the main instructions stay concise.

By default, Codex reads global guidance from:

```text
~/.codex/AGENTS.md
```

If `CODEX_HOME` is set, use that directory instead:

```bash
echo "$CODEX_HOME"
```

## Create Files

Create the global context files:

```bash
mkdir -p ~/.codex
nano ~/.codex/AGENTS.md
nano ~/.codex/OPINIONS.md
nano ~/.codex/VOICE.md
```

Use `AGENTS.md` for durable instructions that apply broadly.
Examples include testing discipline, commit-message conventions, Markdown formatting rules, protected files, and when to consult reference files.

Use `OPINIONS.md` for durable technical opinions.
Examples include preferred tech stacks, architecture principles, programming paradigms, testing philosophy, deployment preferences, and design tradeoffs.

Use `VOICE.md` for communication style.
Examples include how the person writes PR descriptions, issues, comments, reviews, release notes, public posts, and internal updates.

## Suggested AGENTS.md Template

```md
# AGENTS.md

## Person's Agent Instructions

These are common instructions for the person's agents across all projects.

### General Guidelines

- Never introduce the em dash "—" in newly written prose.
  Use plain dash "-" instead.
  Preserve existing em dashes in quoted text, external content, generated files, and code unless explicitly asked to edit them.
- When writing commit messages, never auto-add your agent name as co-author.
- Never manually modify `CHANGELOG.md` files or any files that are marked as auto-generated.
- When writing or substantially editing long Markdown files, put each full sentence on its own physical line.
  Preserve normal Markdown structure, but avoid wrapping multiple sentences onto one physical line.
- When making technical decisions, do not optimize for short-term implementation speed.
  Prefer quality, simplicity, robustness, scalability, and long-term maintainability.
  Still avoid unnecessary complexity.
- When doing bug fixes for user-facing behavior, start by reproducing the bug as close to the end-user path as practical.
  For non-user-facing bugs, create the smallest reliable reproduction that proves the issue.
- When end-to-end testing a product, be picky about the UI you see and hold a high standard for visual quality.
  If something clearly looks off, call it out.
  Fix it when it is blocking, low-risk, explicitly within scope, or directly relevant to the requested work.
- Apply the same high standard to engineering excellence.
  If you see lint failures, test failures, or test flakiness, call them out.
  Fix them when they are blocking, low-risk, explicitly within scope, or directly relevant to the requested work.

### Person's Opinions

When working on something that would benefit from being informed by the person's viewpoints, read `~/.codex/OPINIONS.md` to understand their stance on tech stacks, architecture design patterns, and programming paradigms.

### Voice Profile

When talking or posting on behalf of the person using their identity, such as PR descriptions, issues, or comments, read `~/.codex/VOICE.md` to see how they communicate and format text.

### Protected Reference Files

Treat `~/.codex/OPINIONS.md` and `~/.codex/VOICE.md` as read-only reference files by default.
Read them when relevant.
Do not edit, rewrite, delete, rename, or move them unless the person explicitly asks for that specific file to be changed in the current conversation.
If a requested change appears to require updating either file, explain the proposed change first and ask for confirmation before editing.
```

## Suggested OPINIONS.md Template

```md
# OPINIONS.md

This file records the person's durable technical opinions for agents to consult when a task would benefit from those viewpoints.

## Technical Preferences

- Add preferred languages, frameworks, libraries, and runtimes here.

## Architecture Preferences

- Add architecture principles and design preferences here.

## Engineering Standards

- Add testing, deployment, reliability, and maintainability expectations here.
```

## Suggested VOICE.md Template

```md
# VOICE.md

This file records how the person communicates when an agent writes on their behalf.

## Tone

- Add tone preferences here.

## Formatting

- Add formatting conventions here.

## Examples

- Add short examples of preferred writing here.
```
