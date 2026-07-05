# Agent Operating Model

AI agents need operating rules that live inside the repository. A prompt in a chat window is useful, but it is temporary. Repo instructions create durable memory that every future session can reuse.

## What The Agent Should Know

Every serious project should tell the agent:

- What the project does.
- Which branch and repository are the source of truth.
- Which files contain architecture and deployment notes.
- Which file maps source paths to deployed resources.
- How to run tests, builds, and local servers.
- How deployment works.
- Which deploy target or resource module a change affects.
- What must never be committed.
- How to update changelogs.
- How to treat AWS resources and credentials.

## Working Rules

The agent should:

- Check Git status before editing.
- Read nearby code before making assumptions.
- Keep changes scoped to the request.
- Avoid overwriting user changes.
- Prefer existing project patterns over inventing new ones.
- Run relevant checks before finishing.
- Select the smallest safe check and deploy scope when the project defines routing rules.
- Commit focused changes when asked.
- Update AI-visible changelogs for meaningful changes.
- Document manual cloud changes until they are moved into infrastructure-as-code.

## Why This Matters

AI coding sessions are discontinuous. A future session may not remember why a resource exists, why an architecture decision was made, or which deploy script should be used.

The repo should carry that context:

- `AGENTS.md` for standing instructions.
- `AGENT_WORKFLOW.md` for the operating checklist.
- `AI_CHANGELOG.md` for agent-created history.
- `docs/` for architecture and deployment context.
- `docs/architecture-resource-map.md` or equivalent for source path to resource ownership.
- `scripts/` for repeatable actions.
- `scripts/select-*` style routing helpers when CI or deployment can be scoped safely.

Good AI workflow design is mostly about reducing ambiguity.
