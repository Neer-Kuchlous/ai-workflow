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

## Modular Deployment Pattern

A monolithic deploy command is useful at first because it creates one repeatable path. It becomes a liability when every change is forced through the same broad production workflow.

A better mature shape is:

- Keep one full deploy command for bootstrap and recovery.
- Split cloud resources into modules by ownership and lifecycle.
- Give each module a named deploy target.
- Keep a source path to deploy target map in the repo.
- Let agents deploy only the changed surface when the mapping is unambiguous.
- Make unknown or shared workflow changes fall back to a conservative full path.

This keeps repeatability while reducing blast radius.

## Path-Routed Checks Pattern

CI should match the same boundaries as deployment.

Examples:

- API changes run API syntax checks and API tests.
- Frontend changes run typecheck, build, and frontend tests.
- Script or infrastructure changes run script tests.
- Dependency changes are classified by which workspace or runtime closure they affect.
- Docs-only changes can skip code checks.

The important part is not skipping work for speed. The important part is making the project structure legible enough that the right work can be selected automatically.

## Resource Map Pattern

Every production project should have a human-readable map that answers:

- Which source paths own which runtime surfaces?
- Which infrastructure module owns each cloud resource?
- Which deploy target updates that surface?
- Which resources are long-lived and should be retained?
- Which generated or AI-assisted artifacts live outside Git?

This map is especially useful for agents because it converts architecture from tribal memory into executable context.

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

If the repository is vague, the cloud state is undocumented, and deployment is manual, agents inherit that confusion. If the system has clear instructions, modular resources, repeatable deployment, and explicit security boundaries, agents become much more useful.
