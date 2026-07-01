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
