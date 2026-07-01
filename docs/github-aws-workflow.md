# GitHub + AWS Workflow

The workflow treats GitHub as the canonical source of truth and AWS as deployment state.

That distinction matters. If AWS contains hand-built resources that are not documented or encoded, future AI sessions cannot reason about the system safely.

## GitHub Responsibilities

GitHub should contain:

- Application source code.
- Agent instructions.
- Architecture docs.
- Deployment docs.
- Infrastructure definitions.
- Changelogs.
- Scripts used for builds, deploys, smoke checks, and cleanup.

GitHub should not contain:

- AWS access keys.
- OAuth client secrets.
- Private certificates.
- Local `.env` files.
- Personal access tokens.
- Raw internal exports that are not meant to become project source.

## AWS Responsibilities

AWS should provide managed runtime infrastructure:

- Static hosting.
- Backend compute.
- Databases.
- Object storage.
- IAM roles and policies.
- DNS and certificates.
- Logs and monitoring.

The ideal pattern is to create durable AWS resources through infrastructure-as-code and deployment scripts. Manual changes should be documented immediately and then moved into code later.

## Agent Workflow

For a meaningful change, the agent should usually:

1. Inspect repo status.
2. Read the relevant docs and code.
3. Make a scoped change.
4. Run checks.
5. Update `AI_CHANGELOG.md`.
6. Commit the change.
7. Deploy only when the change affects runtime, infrastructure, data, or config.
8. Smoke-test the deployed result.
9. Leave enough context for the next session.

## Project Isolation

Each serious project should have its own AWS account or clearly isolated environment.

The reason is not just security. It makes AI-assisted work less risky:

- Fewer unrelated resources in scope.
- Cleaner mental model.
- Lower blast radius.
- Easier cleanup.
- Easier cost tracking.
- Better deployment documentation.
