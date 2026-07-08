# Template

## Source Of Truth

GitHub is the source of truth for code.
AWS is the source of truth for data.

## Recommended Project Files

Every project that uses this AI workflow should start with a small set of root files that make the codebase easy for agents and humans to understand.
These files should explain how to work in the repo, what the system does, where resources live, why major decisions were made, and what work is currently active.

Recommended root structure:

```text
AGENTS.md
ARCHITECTURE.md
RESOURCE_MAP.md
TASK_LIST.md
template.md
review.md

ADRS/
  README.md
  0001-example-decision.md

AI_RESOURCES/
  Skills/
  Scripts/
  Templates/
```

## Root Files

`AGENTS.md` tells agents how to interact with the codebase.
It should include repo-specific rules, test commands, coding conventions, protected files, review expectations, and workflow notes.

`ARCHITECTURE.md` explains the major architecture of the codebase.
It should describe the main services, modules, data flow, external dependencies, deployment shape, and important boundaries.

`RESOURCE_MAP.md` explains how to find and access project resources.
It should document AWS accounts, regions, profiles, resource names, ARNs, dashboards, logs, buckets, queues, databases, deployment environments, and safe access commands.
It must not contain secrets, tokens, passwords, private keys, or raw secret values.

`TASK_LIST.md` is the shared working list for agents and humans.
It should track active work, owners, status, branches, blockers, and completed tasks.
It should stay lightweight and structured so agents can update it reliably.

`review.md` documents the review gates agents should run after each slice of progress.
It should define integration checks, adversarial review, local verification, full review, CI/CD checks, E2E tests, and failure handling.

`template.md` documents the reusable project template and source-of-truth rules.
It should stay outside the `setup/` folder because it describes how projects should be initialized, not how to install the local environment.

## Modular Build And Deploy Boundaries

Projects should keep frontend, backend, data, infrastructure, and shared-code paths modular.
Editing one area should not force unrelated areas to be rebuilt, redeployed, or reviewed unless the change affects shared contracts or shared dependencies.
This keeps agent work smaller, review evidence clearer, and deployments safer.

Each project should define its major paths explicitly.
Common path groups include:

- Frontend application paths.
- Backend service paths.
- Data, schema, and migration paths.
- Infrastructure paths.
- Shared package, shared type, and generated contract paths.
- Documentation and template paths.

Use path-routed CI/check selection when practical.
Frontend-only changes should run frontend checks.
Backend-only changes should run backend checks.
Data or migration changes should run data safety checks.
Shared contract changes should run all affected downstream checks.

Use path-routed deploy target selection when practical.
Frontend-only changes should not redeploy backend services unless shared contracts, shared packages, build configuration, or runtime configuration changed.
Backend-only changes should not redeploy frontend artifacts unless the user-facing contract changed.
Data and infrastructure changes should route to the deployment targets that own those resources.

Keep selector logic in tested scripts instead of burying it only in CI YAML.
The scripts that decide which checks or deploy targets to run should have tests of their own.
Broken routing can silently skip important validation, so selector scripts should be treated as production code.

Make routing dependency-aware.
Changes to lockfiles, package manifests, dependency manifests, build configuration, generated clients, shared schemas, or shared types should usually trigger broader checks.
Examples include `package-lock.json`, `pnpm-lock.yaml`, `yarn.lock`, `requirements.txt`, `poetry.lock`, `Cargo.lock`, `go.mod`, and `go.sum`.

Use conservative fallback behavior.
If a change cannot be confidently classified, run the broader check set and choose the broader deploy path.
Unknown, shared, or cross-cutting changes should favor safety over speed.

Use hash-based deploy skips only as an optional optimization.
Skipping deploys for unchanged frontend, API, or infrastructure artifacts can save time, but only after the artifact hash includes all meaningful inputs.
The hash should account for source files, dependency files, build configuration, generated files, and relevant environment-specific configuration.
If the project cannot prove the hash is complete, deploy normally.

Serialize production deploys that write to the same environment.
Two production deploys should not mutate the same stack, database, service, bucket, or routing layer at the same time unless the deployment system is explicitly designed for that concurrency.

Run post-deploy smoke checks.
After deployment, verify that the deployed target is alive and that its most important user or service path still works.
Smoke checks should be small, fast, and specific enough to catch broken deployments.

CI and deploy systems should explain routing decisions in human-readable output.
Agents should be able to read why a workflow selected certain checks, skipped others, selected a deploy target, skipped a deploy, or fell back to a broader path.
This makes automated review easier and prevents agents from guessing why CI behaved a certain way.

## Architecture Decision Records

Use `ADRS/` for Architecture Decision Records.
Prefer a folder of individual ADR files instead of one large architecture decision file.
This keeps decisions easy to review, link, and update over time.

Each ADR should explain:

- The decision.
- The status.
- The context.
- The alternatives considered.
- The consequences.
- Any follow-up work.

Example:

```text
ADRS/
  README.md
  0001-use-lambda-for-api-handlers.md
  0002-store-uploaded-assets-in-s3.md
```

## AI Resources

Use `AI_RESOURCES/` for reusable assets that help agents work inside the project.

`AI_RESOURCES/Skills/` contains repo-specific skills for repeatable specialized tasks.
Use this for project workflows that are too specific for global skills.

`AI_RESOURCES/Scripts/` contains scripts for deployment, testing, verification, maintenance, migrations, and other repeatable operations.
Scripts should be documented and safe to run from the repo root when possible.

`AI_RESOURCES/Templates/` contains reusable templates for pages, prompts, documents, pull requests, issues, reports, or generated artifacts.
Templates should be concrete enough that agents can reuse them without guessing.

Recommended starter templates:

- `AGENTS.md`.
- `AGENT_WORKFLOW.md`.
- `ARCHITECTURE.md`.
- `RESOURCE_MAP.md`.
- `TASK_LIST.md`.
- `ADR.md`.
- `browser-credential-boundary.md`.
- `review-evidence.md`.

## Task List Format

Keep `TASK_LIST.md` structured.
A simple format is easier for agents to update correctly than free-form notes.

Recommended format:

```md
# Task List

## Active

- [ ] Add billing dashboard
  - Owner: codex
  - Status: in progress
  - Branch: feature/billing-dashboard
  - Notes: Waiting on RESOURCE_MAP update.

## Done

- [x] Configure AWS profiles
```

## Resource Map Safety

`RESOURCE_MAP.md` should point to resources without exposing secrets.
It can include AWS profile names, regions, account aliases, ARNs, console links, CLI commands, and runbooks.
It should never include access keys, session tokens, OAuth tokens, passwords, private keys, database credentials, or plaintext secret values.
