# gh-axi

`gh-axi` is an AI-oriented wrapper around GitHub CLI.
It provides compact, structured output for GitHub tasks such as issues, pull requests, workflow runs, releases, repositories, labels, Actions secrets, and Actions variables.

Use `gh-axi` for exploratory GitHub agent work.
Use raw `gh --json` only when the exact fields needed are already known or when `gh-axi` does not support the operation.

## Prerequisites

- GitHub CLI is installed.
- `gh auth status -h github.com` succeeds.
- Node and npm are installed so `npx` can run the wrapper.

## Test

Test the wrapper from any folder:

```bash
npx -y gh-axi
```

## Install Skill

Install the Codex skill globally so Codex can discover `gh-axi` across projects:

```bash
npx skills add kunchenguid/gh-axi --skill gh-axi -g
```

When prompted for additional agents, select none unless those tools are actively used.
Codex is included by default.

After installation, restart Codex and verify that the skill appears:

```text
/skills
```

The skill should be installed at:

```text
~/.agents/skills/gh-axi
```

Do not alias `gh` to `gh-axi`.
`gh-axi` is the preferred AI-facing wrapper, but it is not a complete replacement for every GitHub CLI command.
