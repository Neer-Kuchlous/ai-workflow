# Lavish

Lavish Editor turns local HTML artifacts into browser review surfaces.
Use it when a plan, comparison, diagram, table, code diff, report, or UI proposal is easier to review visually than in chat.
Lavish lets the user annotate elements or selected text in the browser, then sends those prompts back to Codex through a polling command.

## Prerequisites

- Node and npm are installed so `npx` can run Lavish.
- Codex is installed and can run from the terminal.

## Install Skill

Install the Codex skill globally so Codex can discover the Lavish workflow across projects:

```bash
npx -y skills add kunchenguid/lavish-axi --skill lavish --agent codex -g -y
```

Verify the CLI:

```bash
npx -y lavish-axi --help
```

After installation, restart Codex and verify that the skill appears:

```text
/skills
```

Lavish is a Codex skill, not a built-in slash command.
Do not expect `/lavish` to work unless a separate custom prompt has been created.

To start a Lavish workflow, mention the skill with `$lavish` in the prompt:

```text
$lavish Create a visual review artifact for this plan and keep polling until I click Send & End.
```

If skill mentions are not available in the current Codex surface, select `lavish` from `/skills` or tell Codex directly:

```text
Use the lavish skill for this and keep polling until I click Send & End.
```

The skill should be installed at:

```text
~/.agents/skills/lavish
```

## Review Workflow

Create Lavish artifacts in the current project under `.lavish/` by default:

```text
.lavish/example.html
```

Open or resume a Lavish review session:

```bash
npx -y lavish-axi .lavish/example.html
```

Poll for browser feedback:

```bash
npx -y lavish-axi poll .lavish/example.html
```

Keep the poll running during the review.
The poll may stay silent until the user sends feedback, ends the session, or the browser reports layout warnings.
Do not stop polling during an active review unless the user is done or the session needs to be reset.

When feedback arrives, Codex edits the HTML file locally.
Lavish does not directly patch files from the browser by itself.
The browser sends selected-element context and user prompts to Codex, Codex applies the file changes, and Lavish live-reloads the page.

After applying feedback, reply inside the Lavish sidebar and continue polling:

```bash
npx -y lavish-axi poll .lavish/example.html --agent-reply "Applied the requested change."
```

End a review session when finished:

```bash
npx -y lavish-axi end .lavish/example.html
```

Stop the local Lavish server:

```bash
npx -y lavish-axi stop
```

## Export And Share

Export a portable one-file HTML copy when the artifact should be shared without the Lavish server:

```bash
npx -y lavish-axi export .lavish/example.html --out example.exported.html
```

Use `share` only when a public or password-protected hosted URL is intentionally needed:

```bash
npx -y lavish-axi share .lavish/example.html --password "choose-a-password"
```

Lavish share publishes to `ht-ml.app`, which is a third-party hosting service.
Treat shared pages as public unless a password is used.

## Design Guidance

Before creating a Lavish artifact, choose the design source in this order:

1. Use the user's requested look or named design system.
2. Match the subject project's existing design system, CSS variables, component library, or styled pages.
3. If neither applies, use the Lavish fallback design guidance.

Run the design helper for fallback guidance:

```bash
npx -y lavish-axi design
```

For diagrams, tables, comparisons, plans, code views, input collection, or slides, open the relevant playbook before writing the HTML:

```bash
npx -y lavish-axi playbook diagram
npx -y lavish-axi playbook table
npx -y lavish-axi playbook comparison
npx -y lavish-axi playbook plan
npx -y lavish-axi playbook code
npx -y lavish-axi playbook input
npx -y lavish-axi playbook slides
```

During a real Lavish review, Codex should keep the current turn open and continue polling until the user clicks `Send & End` or asks to stop.
If Codex sends a normal final chat response too early, the browser may appear to keep working while the agent is no longer actively applying feedback.
