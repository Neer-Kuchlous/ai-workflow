# Codex Install And Login

Codex is the terminal coding agent for this workflow.
Run Codex from the repository root inside tmux.

## Install

Install Codex with Homebrew:

```bash
brew install --cask codex
```

Verify the install:

```bash
codex --version
```

## Login

Log in with ChatGPT:

```bash
codex login
```

Check login status:

```bash
codex login status
```

Expected result:

```text
Logged in using ChatGPT
```

Alternative API-key login:

```bash
printenv OPENAI_API_KEY | codex login --with-api-key
```

## Start Codex

Start Codex from a project folder:

```bash
cd ~/Code/ai-workflow
codex
```
