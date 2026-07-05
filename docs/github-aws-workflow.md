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
- A resource ownership map that connects source paths, deploy targets, infrastructure modules, and cloud resources.
- Changelogs.
- Scripts used for builds, deploys, deploy-target selection, smoke checks, and cleanup.

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

## Resource Ownership

Cloud resources should be split by lifecycle and ownership instead of bundled into one undifferentiated production deploy.

A practical split is:

- CI/deploy identity: OIDC provider, deploy role, and automation permissions.
- Runtime data: databases, queues, private runtime buckets, retention policies, and migration scripts.
- API runtime: backend compute, API gateway, execution role, logs, and runtime configuration.
- Frontend and edge: static asset bucket, CDN, DNS, TLS, and cache invalidation.
- Development artifacts: private prompt/output archives, generated fixtures, and scratch outputs that should not become app source.

This makes it clear which part of production a change can affect. It also lets agents deploy narrowly without pretending every edit requires a full infrastructure update.

## Deploy Target Routing

When a repository has clear source ownership, deployment can be selected from changed paths.

Examples:

- Frontend source changes should build and publish frontend assets.
- API source changes should package and deploy the backend runtime.
- Runtime data resource templates should update data prerequisites without rebuilding the frontend.
- CI/deploy-role templates should update automation identity only.
- Documentation-only changes should not deploy.
- Shared deploy machinery should use a conservative full deploy path.

The routing logic should live in code, have tests, and produce an explanation that agents can read in CI logs. Unknown paths should default to conservative behavior.

## Agent Workflow

For a meaningful change, the agent should usually:

1. Inspect repo status.
2. Read the relevant docs and code.
3. Make a scoped change.
4. Run checks.
5. Update `AI_CHANGELOG.md`.
6. Commit the change.
7. Identify the deploy target from the source/resource map.
8. Deploy only when the change affects runtime, infrastructure, data, or config.
9. Prefer the smallest safe deploy target when routing rules exist.
10. Smoke-test the deployed result.
11. Leave enough context for the next session.

## Project Isolation

Each serious project should have its own AWS account or clearly isolated environment.

The reason is not just security. It makes AI-assisted work less risky:

- Fewer unrelated resources in scope.
- Cleaner mental model.
- Lower blast radius.
- Easier cleanup.
- Easier cost tracking.
- Better deployment documentation.
