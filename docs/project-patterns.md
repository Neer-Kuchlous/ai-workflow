# Project Patterns

These notes describe generic project patterns that make AI-assisted software work more reliable.

## Disciplined Application Pattern

A project is easier for agents to work on when the boundaries are explicit:

- GitHub is the source of truth.
- Repo-level agent instructions define expected behavior.
- AI changelog entries preserve important context.
- The frontend has a clear build and deployment path.
- The backend owns privileged operations.
- Cloud resources are documented or managed through infrastructure-as-code.
- Deployment scripts and smoke checks are repeatable.
- Browser code calls backend APIs rather than cloud services directly.

The lesson is that agents perform better when the project has clear boundaries, scripts, and documentation.

## Cleanup And Rebuild Pattern

Some projects accumulate architecture that is technically functional but no longer worth carrying forward.

In that situation, the useful move can be to separate durable assets from replaceable implementation details.

Durable assets might include:

- Product requirements.
- Domain knowledge.
- User-facing constraints.
- Stable public configuration.
- Lessons from previous implementation work.

Replaceable implementation details might include:

- Old deployment scripts.
- Hand-wired cloud resources.
- Unused data stores.
- Stale backend endpoints.
- Build systems that no longer match the product direction.

The goal is not deletion for its own sake. The goal is to remove architectural drag so the next implementation can have clearer boundaries.

## General Lesson

AI agents are not a replacement for architecture. They amplify the architecture already present.

If the repository is vague, the cloud state is undocumented, and deployment is manual, agents inherit that confusion. If the system has clear instructions, repeatable deployment, and explicit security boundaries, agents become much more useful.
