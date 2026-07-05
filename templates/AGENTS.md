# Agent Instructions

This repository is the source of truth for the project.

## Rules

- Check `git status` before editing.
- Read relevant docs and nearby code before making changes.
- Keep changes scoped to the user's request.
- Do not overwrite user changes.
- Do not commit secrets, `.env` files, credentials, or local dumps.
- Keep browser clients untrusted.
- Never expose AWS credentials or backend secrets to frontend code.
- Use backend IAM roles, API routes, or presigned URLs for AWS access.
- Update `AI_CHANGELOG.md` for meaningful AI-assisted changes.
- Run relevant checks before finishing.
- Use the repo's source-to-resource map to choose checks and deploy targets when one exists.

## Deployment

- Treat AWS as deployment state.
- Prefer infrastructure-as-code for durable resources.
- Split durable resources by ownership and lifecycle when the project is large enough.
- Document manual AWS changes immediately.
- Deploy only when runtime, infrastructure, data, or configuration changed.
- Prefer the smallest safe deploy target, and use the full deploy path for shared or unclear changes.
