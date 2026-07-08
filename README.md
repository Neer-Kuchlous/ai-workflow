# AI Workflow

This repository is a reusable workflow template for AI-assisted software projects.
It collects the project structure, setup docs, review gates, and starter templates that help coding agents work safely across code, infrastructure, data, and deployment workflows.

## Core Ideas

GitHub is the source of truth for code.
AWS is the source of truth for data.
Agents need durable repo instructions, not only one-off prompts.
Architecture, resource maps, review evidence, and task lists should live in the repo so future sessions can continue without guessing.

## Main Files

- [template.md](template.md) describes the recommended project shape.
- [review.md](review.md) defines the A1, A2, and A3 review gates.
- [setup/](setup/README.md) contains modular environment setup instructions.
- [AI_RESOURCES/Templates/](AI_RESOURCES/Templates/) contains starter files for new projects.

## Recommended Project Shape

New projects can start from this shape:

```text
AGENTS.md
ARCHITECTURE.md
RESOURCE_MAP.md
TASK_LIST.md
template.md
review.md

ADRS/
  README.md
  0001-example-decision.md

AI_RESOURCES/
  Skills/
  Scripts/
  Templates/
```

The exact stack can change.
The important part is that the repo explains how source paths map to runtime surfaces, deploy targets, checks, and cloud resources.

## How To Use

1. Follow the relevant setup modules in [setup/](setup/README.md).
2. Use [template.md](template.md) as the project initialization guide.
3. Copy starter templates from [AI_RESOURCES/Templates/](AI_RESOURCES/Templates/) into a new project.
4. Customize the copied files for the project stack, AWS accounts, deploy targets, and test commands.
5. Use [review.md](review.md) after each slice of progress.
