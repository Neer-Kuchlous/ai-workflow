# Agent Workflow

Use this checklist for meaningful AI-assisted changes.

## Start

1. Check the current branch and working tree.
2. Read the user request carefully.
3. Inspect relevant docs, code, scripts, and infrastructure files.
4. Identify whether the change affects local code only or deployed infrastructure.

## Change

1. Make the smallest coherent change.
2. Follow existing project patterns.
3. Avoid unrelated refactors.
4. Keep secrets out of Git.
5. Update docs when architecture or deployment behavior changes.

## Verify

1. Run the most relevant tests, builds, linters, or smoke checks.
2. If deployment changed, verify the deployed endpoint or resource.
3. Record any tests that could not be run.

## Finish

1. Update `AI_CHANGELOG.md` for meaningful work.
2. Commit with a clear message if the user asked for repo changes.
3. Push or deploy only when requested or when the project workflow requires it.
4. Leave a concise summary with changed files, verification, and remaining risks.
