# AI Workflow

This repository documents a practical workflow for making AI coding agents useful in real software projects.

The core idea is simple:

> AI agents work better when GitHub, cloud infrastructure, deployment routing, and repo-level instructions are designed as one system.

This is not an AI hype repository. It is a small public reference for how I structure projects so AI agents can make changes, leave context, interact with production infrastructure safely, and hand off work to the next session without losing the plot.

## Principles

- GitHub is the source of truth.
- AWS is deployment state.
- AI agents need durable repo instructions, not only one-off prompts.
- Meaningful AI-authored changes should leave a changelog entry.
- Infrastructure should be repeatable, documented, and split by resource ownership.
- Deploy scripts should support scoped targets instead of forcing every change through one full production path.
- CI and deployment should be routed from changed paths when the repository is structured clearly enough to do that safely.
- The browser should never receive AWS credentials.
- Project isolation matters: separate AWS accounts or environments reduce blast radius.
- Architecture and resource maps are working memory for future agents.

## What Is In This Repo

- [Agent operating model](docs/agent-operating-model.md)
- [GitHub + AWS workflow](docs/github-aws-workflow.md)
- [Modular deployment and resource ownership](docs/modular-deployment-and-resource-ownership.md)
- [Scalable web app architecture pattern](docs/scalable-web-app-architecture.md)
- [Browser credential boundary](docs/browser-credential-boundary.md)
- [AI changelog discipline](docs/changelog-discipline.md)
- [General project patterns](docs/project-patterns.md)
- [Reusable templates](templates/)
- [LinkedIn GPT Pro handoff](linkedin/2026-07-modular-agent-workflow/)

## Reference Shape

A production app using this workflow usually looks like this:

- TypeScript frontend
- Backend API boundary
- Static frontend hosting through S3 and CloudFront
- API Gateway and Lambda for backend routes
- DynamoDB or S3 for managed data storage
- IAM roles for backend AWS access
- Presigned URLs for browser uploads/downloads when needed
- CloudFormation, CDK, Terraform, or another infrastructure-as-code tool for durable resources
- Separate infrastructure modules or stacks for CI identity, runtime data, API hosting, frontend/edge hosting, and private development artifacts
- Path-routed CI/deploy selectors that map changed files to the smallest safe checks and deploy targets

The exact stack can change. The important part is that agents can understand the repo, make scoped edits, run checks, update the changelog, and deploy through repeatable scripts with clear target boundaries.

## Intended Use

Use this repository as context for:

- AI coding agent setup
- GitHub repo structure
- AWS deployment discipline
- Public explanation of an AI-assisted engineering workflow
- LinkedIn or blog posts about building reliable AI development loops
