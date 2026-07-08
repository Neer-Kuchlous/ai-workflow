# GitHub HTTPS Authentication

Use GitHub HTTPS authentication for this workflow, not SSH authentication.
GitHub is the source of truth for code, so the machine needs authenticated access for `git` and `gh` commands.

## Install GitHub CLI

```bash
brew install gh
```

## Login

Log in through the browser:

```bash
gh auth login --web --git-protocol https
```

Recommended choices during login:

- Host: `GitHub.com`
- Protocol for Git operations: `HTTPS`
- Authentication method: browser login

Configure Git to use GitHub CLI as the credential helper:

```bash
gh auth setup-git
```

Check authentication status:

```bash
gh auth status -h github.com
```

Verify GitHub API access:

```bash
gh repo list --limit 5
```

Verify `git` HTTPS access from a repository:

```bash
git ls-remote origin HEAD
```

## Repository Remotes

Repository remotes should use HTTPS URLs:

```bash
git remote -v
```

Expected shape:

```text
origin  https://github.com/OWNER/REPO.git (fetch)
origin  https://github.com/OWNER/REPO.git (push)
```

If a repository is using SSH, switch it to HTTPS:

```bash
git remote set-url origin https://github.com/OWNER/REPO.git
```

Do not paste GitHub tokens into setup docs, `AGENTS.md`, committed `.env` files, or prompts.
Let GitHub CLI and the macOS credential store manage the token.
