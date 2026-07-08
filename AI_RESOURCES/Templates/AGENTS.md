# AGENTS.md

This file tells agents how to work in this repository.
Keep it specific to the project.
Update it when the workflow, checks, deployment process, protected files, or architecture rules change.

## Project Context

- Read `ARCHITECTURE.md` before making architecture-sensitive changes.
- Read `RESOURCE_MAP.md` before touching infrastructure, deployment, data, cloud resources, or path-routed checks.
- Read `review.md` before finalizing a meaningful change.
- Use `TASK_LIST.md` to understand active work and unresolved follow-ups.
- Use `ADRS/` to understand why major architectural decisions were made.

## Working Rules

- Check `git status` before editing.
- Read relevant docs and nearby code before making changes.
- Keep changes scoped to the user's request.
- Do not overwrite user changes.
- Prefer existing project patterns over new abstractions.
- Do not commit secrets, `.env` files, credentials, tokens, private keys, or local dumps.
- Keep browser clients untrusted.
- Never expose AWS credentials or backend secrets to frontend code.
- Use backend IAM roles, API routes, or presigned URLs for AWS access.
- Run relevant checks before finishing.
- Use the repo's source-to-resource map to choose checks and deploy targets when one exists.
- Use conservative full checks for shared, unknown, or cross-cutting changes.

## Review Rules

- Use the A1, A2, and A3 gates in `review.md` after each slice of progress.
- If a review gate fails, investigate, fix the issue when practical, and rerun the relevant gate.
- Record what was checked, what passed, what failed, and what remains risky.

## Deployment Rules

- Treat GitHub as the source of truth for code.
- Treat AWS as the source of truth for data and runtime state.
- Prefer infrastructure-as-code for durable resources.
- Split durable resources by ownership and lifecycle when the project is large enough.
- Document manual AWS changes immediately and move them into code later.
- Deploy only when runtime, infrastructure, data, or configuration changed.
- Prefer the smallest safe deploy target when routing rules exist.
- Use the full deploy path for shared or unclear changes.
- Run post-deploy smoke checks after deployment.
