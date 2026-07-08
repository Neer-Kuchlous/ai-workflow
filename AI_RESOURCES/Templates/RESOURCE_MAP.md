# Resource Map

This file maps source paths to runtime surfaces, AWS resources, deploy targets, and checks.
It is operational memory for humans and agents.
Keep it current whenever resources, deployment targets, or ownership boundaries change.

## Source Of Truth

GitHub is the source of truth for code.
AWS is the source of truth for data and runtime state.

## Source Path Ownership

| Path | Owner | Checks | Deploy Target | Notes |
| --- | --- | --- | --- | --- |
| `apps/web/` | Frontend | frontend typecheck, frontend tests, frontend build | frontend | Browser code and public assets |
| `apps/api/` | Backend API | API tests, typecheck, lint | api | Privileged operations and service logic |
| `data/` | Data | migration checks, data validation | data | Durable runtime data and migrations |
| `infra/ci/` | CI identity | infrastructure checks | ci | OIDC providers, deploy roles, automation permissions |
| `infra/api/` | API infrastructure | infrastructure checks, API smoke tests | api or infra | API gateway, compute, logs, runtime config |
| `infra/frontend/` | Frontend infrastructure | infrastructure checks, frontend smoke tests | frontend or infra | Static hosting, CDN, DNS, certificates |
| `scripts/` | Tooling | script tests | full when shared | Deploy, routing, smoke, and maintenance scripts |
| `docs/` | Documentation | docs checks | none | No deploy unless docs are published runtime artifacts |

## Deploy Targets

| Target | Purpose | Typical Inputs | Smoke Check |
| --- | --- | --- | --- |
| `ci` | Automation identity and deploy permissions | `infra/ci/` | Verify CI can authenticate |
| `data` | Durable data resources and migrations | `data/`, `infra/data/` | Verify data resource availability |
| `api` | Backend runtime | `apps/api/`, API infra, shared backend dependencies | Call health endpoint or key API route |
| `frontend` | Browser build and public assets | `apps/web/`, frontend infra, shared frontend dependencies | Load key page through public URL |
| `infra` | Shared hosting, routing, DNS, certificates, or edge resources | `infra/` | Verify affected runtime surface |
| `full` | Bootstrap, recovery, unknown, or shared changes | Any cross-cutting change | Run all relevant smoke checks |

## AWS Resource Inventory

| Environment | Account Alias | Region | Resource | Owner | Managed By | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| dev | example-dev | us-east-1 | example-resource | api | IaC | Replace with real resource |
| prod | example-prod | us-east-1 | example-resource | api | IaC | Replace with real resource |

## Routing Rules

- Frontend-only changes should run frontend checks and select the frontend deploy target.
- Backend-only changes should run backend checks and select the API deploy target.
- Data and migration changes should run data safety checks and select the data deploy target.
- Infrastructure changes should select the target that owns the changed infrastructure module.
- Shared package, shared type, generated client, lockfile, dependency, or build configuration changes should trigger broader checks.
- Unknown or cross-cutting changes should fall back to the full check and deploy path.
- Docs-only changes should not deploy unless the docs are themselves published runtime artifacts.

## Secret Safety

Do not put secrets in this file.
Allowed content includes account aliases, profile names, regions, ARNs, resource names, console links, and safe CLI commands.
Forbidden content includes access keys, session tokens, OAuth client secrets, passwords, private keys, database credentials, and plaintext secret values.

## Browser Credential Boundary

The browser should never receive AWS credentials.
Browser code may receive public configuration such as API base URLs, OAuth client IDs, feature flags that are not sensitive, and public asset URLs.
Backend services should own AWS access through IAM roles or equivalent service identity.
Use presigned URLs when the browser needs direct file upload or download access.
