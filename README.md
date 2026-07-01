# AI Workflow

This repository documents a practical workflow for making AI coding agents useful in real software projects.

The core idea is simple:

> AI agents work better when GitHub, cloud infrastructure, deployment scripts, and repo-level instructions are designed as one system.

This is not an AI hype repository. It is a small public reference for how I structure projects so AI agents can make changes, leave context, interact with production infrastructure safely, and hand off work to the next session without losing the plot.

## Principles

- GitHub is the source of truth.
- AWS is deployment state.
- AI agents need durable repo instructions, not only one-off prompts.
- Meaningful AI-authored changes should leave a changelog entry.
- Infrastructure should be repeatable and documented.
- The browser should never receive AWS credentials.
- Project isolation matters: separate AWS accounts or environments reduce blast radius.
- Architecture docs are working memory for future agents.

## What Is In This Repo

- [Agent operating model](docs/agent-operating-model.md)
- [GitHub + AWS workflow](docs/github-aws-workflow.md)
- [Scalable web app architecture pattern](docs/scalable-web-app-architecture.md)
- [Browser credential boundary](docs/browser-credential-boundary.md)
- [AI changelog discipline](docs/changelog-discipline.md)
- [General project patterns](docs/project-patterns.md)
- [Reusable templates](templates/)
- [Prompt for GPT Pro](linkedin/GPT_PRO_LINKEDIN_PROMPT.md)

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

The exact stack can change. The important part is that agents can understand the repo, make scoped edits, run checks, update the changelog, and deploy through repeatable scripts.

## Intended Use

Use this repository as context for:

- AI coding agent setup
- GitHub repo structure
- AWS deployment discipline
- Public explanation of an AI-assisted engineering workflow
- LinkedIn or blog posts about building reliable AI development loops
