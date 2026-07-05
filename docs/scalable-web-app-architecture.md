# Scalable Web App Architecture

The architecture pattern I prefer is intentionally boring. It gives AI agents a clear system to reason about and gives humans a repeatable path from local development to production.

## Frontend

- TypeScript
- Vite or another fast build tool
- Browser calls the backend API, not AWS directly
- Public config only, such as API base URL or OAuth client ID
- No secrets in frontend code

## Backend

- API boundary for application logic
- Backend owns AWS access through IAM roles
- Auth verification happens server-side
- Data validation happens server-side
- Presigned URLs are issued when the browser needs direct object upload/download

## Hosting

A common production shape:

- S3 stores built frontend assets.
- CloudFront serves the frontend over HTTPS.
- API Gateway routes `/api/*` to backend compute.
- Lambda or containers run the backend.
- DynamoDB stores structured app data.
- S3 stores private artifacts and uploads.
- Private buckets store runtime cache, generated fixtures, or AI-assisted work packets when those artifacts should not be committed.
- Route53 manages DNS.
- ACM manages TLS certificates.

## Why TypeScript Helps

TypeScript creates structure for both humans and agents:

- API contracts are easier to discover.
- Refactors are safer.
- Build errors catch many mistakes before deployment.
- Shared types can reduce frontend/backend drift.

The goal is not to make the stack complex. The goal is to make the boundaries explicit.

## Infrastructure-As-Code

Infrastructure should be repeatable. CloudFormation, CDK, Terraform, Pulumi, or another tool can work.

The important properties are:

- A new agent can find the infrastructure definition.
- Changes can be reviewed before being applied.
- Deployment steps are scripted.
- Manual cloud state does not become invisible project knowledge.

## Modular Resource Ownership

Infrastructure-as-code should not become one giant template that every change touches. Split resources by lifecycle and deploy target when the project grows:

- CI identity and deploy roles.
- Runtime data stores and private support buckets.
- Backend/API runtime.
- Frontend hosting, CDN, DNS, and certificates.
- Private development or generated-artifact storage.

The repo should document how these modules depend on each other. For example, the frontend/edge module may need the API origin output, while the API module may need runtime table and bucket names.

## Deploy Routing

Once source ownership is clear, the deploy system can route changes:

- Frontend paths to frontend deploy.
- API paths to API deploy.
- Runtime data templates to data deploy.
- Hosting templates to infrastructure deploy.
- Docs-only paths to no deploy.

Keep the full deploy available, but do not make it the only path.
