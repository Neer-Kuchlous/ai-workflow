# Architecture

This file explains the major architecture of the project.
Keep it practical.
Future agents should be able to read this file and understand the system boundaries before editing code.

## System Overview

Describe what the system does and who uses it.
Include the main runtime surfaces and the most important external dependencies.

## Major Components

| Component | Purpose | Owner Path | Runtime Surface |
| --- | --- | --- | --- |
| Frontend | User interface | `apps/web/` | Browser |
| Backend API | Application logic and privileged operations | `apps/api/` | API service |
| Data | Durable project data and migrations | `data/` | Database or object storage |
| Infrastructure | Cloud resources and deployment configuration | `infra/` | AWS or other cloud provider |

## Frontend Boundary

Describe the frontend framework, build command, public configuration, routing model, and API boundary.
The frontend should not receive AWS credentials, backend secrets, database credentials, or raw secret values.

## Backend Boundary

Describe the backend runtime, API routes, auth verification, validation rules, and AWS access model.
The backend should own privileged AWS access through IAM roles or equivalent service identity.

## Data Boundary

Describe durable stores, schemas, migrations, retention rules, and restore paths.
Document which data lives in AWS or another managed system and which data is committed to Git.

## Infrastructure Boundary

Describe the infrastructure-as-code tool, deploy targets, environments, and dependency order.
Link to `RESOURCE_MAP.md` for exact resources, ARNs, accounts, regions, and deploy commands.

## Deployment Shape

Describe how code moves from local development to production.
Include CI, build artifacts, deploy targets, smoke checks, rollback expectations, and production serialization rules.

## Open Questions

- [ ] Document unresolved architecture questions here.
