# Old To New Workflow Comparison

## Before

The original workflow was already better than ad hoc prompting:

- GitHub was the source of truth.
- AWS was treated as deployment state.
- Agents had repo-level instructions.
- Meaningful AI work left changelog entries.
- Deployments were scripted.
- The browser credential boundary was explicit.
- Architecture docs preserved context between sessions.

The weakness was structural:

- Deployment logic was too centralized.
- One broad production deploy path did too much work.
- AWS resources were not separated enough by lifecycle.
- Agents had to infer which part of production a source change affected.
- Small changes could trigger broad cloud work.
- Runtime data resources, API runtime, frontend hosting, and CI/deploy identity were not legible as separate surfaces.

## After

The newer workflow keeps the original discipline, but makes production ownership explicit:

- The repo has separate app workspaces for frontend and API code.
- Infrastructure is split into smaller modules or stacks.
- Runtime data resources are separated from API hosting.
- Frontend/edge hosting is separated from API runtime.
- CI deploy identity is separated from application infrastructure.
- Development artifacts and generated fixtures are treated differently from runtime source.
- A resource map connects source paths, infrastructure modules, cloud resources, and deploy commands.
- CI checks are selected from changed paths.
- Production deploy targets are selected from changed paths.
- Lockfile changes are classified by dependency closure when possible.
- Artifact hashes skip unchanged package uploads and frontend syncs.
- Production deploy jobs are serialized to prevent overlapping main-branch deploys.
- Post-deploy smoke checks verify key production surfaces.

## Why The Change Matters For AI Agents

Agents need to know the blast radius of their work.

In the old shape, an agent could follow instructions and still have a vague deployment model:

> "Run the deploy script."

In the new shape, the agent gets a clearer operational model:

> "This file changed. It maps to this owner, this check set, this deploy target, and these cloud resources."

That is a major difference. It turns deployment from a ritual into a system the agent can inspect, explain, and use safely.

## Generalizable Takeaway

The lesson is not specific to one stack. It applies to any project with agents touching real production systems:

- Modularize resources by ownership.
- Document the source-to-resource map.
- Encode path routing for checks and deploys.
- Keep conservative fallbacks.
- Test the routing code.
- Make automation explain its decisions.

AI agents are much more useful when the repo tells them where responsibility begins and ends.
