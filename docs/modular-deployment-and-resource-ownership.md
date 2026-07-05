# Modular Deployment And Resource Ownership

The first version of an AI-agent workflow can get surprisingly far with one deploy script. The problem is that a single production path eventually hides too much:

- Agents cannot tell which resources a change affects.
- Small frontend changes can trigger broad infrastructure work.
- API packaging, data prerequisites, DNS/CDN updates, and CI bootstrap logic become coupled.
- Cloud state becomes harder to reason about because ownership is implicit.
- A failed deploy step can block unrelated surfaces.

The fix is not to remove the full deploy path. The fix is to make it an orchestration path over smaller owned targets.

## Target Shape

A mature workflow should define named deploy targets such as:

- `ci`: automation identity, deploy roles, OIDC providers, and CI permissions.
- `data`: durable runtime data stores, private support buckets, lifecycle policies, and migrations.
- `api`: backend runtime packaging, API gateway, backend roles, logs, and runtime configuration.
- `infra`: hosting or edge infrastructure that connects existing runtime surfaces.
- `frontend`: browser build, static asset sync, CDN invalidation, and public config.

The names do not matter. The separation does.

## Source Ownership Map

Keep a repo document that maps source paths to owners and deploy targets.

Example:

```md
| Path | Owner | Deploy target |
| --- | --- | --- |
| apps/web/src/ | Browser frontend | frontend |
| apps/api/server/ | Backend API | api |
| data/runtime/*.json | API runtime data | api |
| infra/runtime-data.* | Runtime data stores | data |
| infra/api.* | API hosting | infra |
| infra/frontend.* | Frontend/edge hosting | infra |
| infra/ci.* | Deploy automation identity | ci |
| scripts/deploy-* | Deploy machinery | full |
```

This table gives agents a practical answer to "what should I deploy after this change?"

## Path-Routed CI And Deploy

When the source map is stable, encode it in a small selector script.

The selector should:

- Read changed paths from Git.
- Return the minimum safe check list or deploy target list.
- Treat docs-only changes as no deploy.
- Treat tests and fixtures as check-only unless they are packaged into runtime.
- Treat shared deploy machinery conservatively.
- Explain each routing decision in machine-readable and human-readable formats.
- Have focused tests for important routing cases.

Use the same mental model for CI and deploy. If the API owns a path, API checks and API deploy should be selected. If the frontend owns a path, frontend checks and frontend deploy should be selected.

## Dependency-Aware Routing

Lockfile changes are easy to over-deploy. A better selector can inspect which dependency closure changed:

- Backend runtime dependencies affect the API package.
- Frontend runtime or build dependencies affect the frontend build.
- Script-only cloud dependencies may affect data or infrastructure tooling.
- Test-only dependencies should run checks but usually should not deploy.

If the selector cannot classify the lockfile safely, it should choose the conservative full path.

## Hash-Based Deploy Skips

Even after a target is selected, the deploy script can avoid cloud churn by hashing built artifacts:

- Hash the packaged API payload before uploading a new artifact.
- Hash the frontend `dist/` output before syncing and invalidating CDN caches.
- Store the last deployed hash in a small deployment-state location.
- Skip upload or invalidation when the artifact is unchanged.

This keeps routing simple while avoiding unnecessary production mutations.

## Agent Benefits

This structure gives agents:

- A smaller blast radius for each change.
- Faster feedback because checks and deploys match the touched surface.
- Better handoffs because resource ownership is documented.
- Safer cloud work because long-lived resources are not mixed with every app deploy.
- Clearer explanations in final responses and changelog entries.

The broader lesson is that AI agents need operational architecture, not just prompts. They work better when the repository tells them which code owns which resources and which command updates each surface.
