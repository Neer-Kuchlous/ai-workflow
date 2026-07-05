# Generalized Workflow Insights

These notes are extracted from a newer AI-agent development workflow and rewritten so they apply to any real software project.

## Core Lesson

AI coding agents do not just need better prompts. They need a repository and deployment system that tells them how the software maps to production.

The first workflow had a useful foundation:

- GitHub as the source of truth.
- AWS as deployment state.
- Repo-level agent instructions.
- Changelogs and architecture docs.
- Repeatable deployment scripts.

The flaw was that too much production behavior was bundled behind one broad deploy path, and the cloud resources were not modular enough for agents to reason about ownership and blast radius.

The newer workflow keeps the durable parts of the original system, but adds clearer operational architecture:

- Resource modules separated by lifecycle and ownership.
- Source paths mapped to deploy targets.
- CI checks selected from changed paths.
- Production deploys selected from changed paths.
- Dependency changes classified by the runtime or workspace they affect.
- Hash-based deploy skips to avoid uploading or invalidating unchanged artifacts.
- Human-readable architecture/resource maps for future agents.

## Transferable Patterns

### 1. Keep One Full Deploy, But Stop Making It The Only Path

A full deploy command is still valuable for bootstrap, recovery, and shared workflow changes. It should not be the default for every code edit.

Better pattern:

- `full`: run everything.
- `frontend`: build and publish browser assets.
- `api`: package and deploy backend runtime.
- `infra`: update hosting or edge templates.
- `data`: update durable runtime data resources.
- `ci`: update automation identity and deploy-role resources.

The exact target names can change. The important part is that production has named surfaces.

### 2. Split Cloud Resources By Ownership

Cloud resources should be grouped by lifecycle and blast radius:

- CI identity and deploy roles.
- Runtime databases and private support buckets.
- API compute, gateway, roles, logs, and runtime config.
- Frontend hosting, CDN, DNS, and certificates.
- Private development artifacts and generated fixtures.

This keeps long-lived resources from being casually mixed with every frontend or API deploy.

### 3. Add A Source Path To Resource Map

Agents should be able to answer:

- Which source paths own the frontend?
- Which source paths own the API?
- Which data files are bundled into runtime?
- Which infrastructure module owns each cloud resource?
- Which deploy target should run after a change?

This can be a simple Markdown table. It becomes operational memory for every future agent session.

### 4. Route CI And Deploys From Changed Paths

Once ownership is explicit, changed files can select checks and deploys:

- API changes run API checks and deploy API.
- Frontend changes run frontend checks and deploy frontend.
- Infra template changes run script/template checks and deploy infra.
- Runtime data resource changes deploy data prerequisites.
- Docs-only changes skip production deploy.
- Shared workflow or unknown paths fall back to a conservative broader path.

The selector logic should be tested. If an agent is going to trust it, it needs to be code, not vibes.

### 5. Treat Lockfiles As Structured Data

Dependency changes should not automatically imply a full deploy.

A mature selector can inspect which dependency closure changed:

- API runtime dependency: deploy API.
- Frontend runtime or build dependency: deploy frontend.
- Script-only AWS dependency: run script checks and deploy affected infrastructure/data surfaces if needed.
- Test-only dependency: run checks, usually no production deploy.

If the script cannot classify the change, it should fall back to the conservative path.

### 6. Add Artifact Hash Skips

Even if a target is selected, the deploy can avoid production churn:

- Hash the backend package before uploading it.
- Hash the frontend build output before syncing it.
- Store the last deployed hash in deploy state.
- Skip uploads, stack updates, or CDN invalidations when output is unchanged.

This makes deploy routing both safer and quieter.

### 7. Make CI Explain Its Routing Decisions

Good automation should tell agents why it selected checks or deploy targets.

Useful outputs:

- Machine-readable values for CI conditionals.
- Human-readable Markdown summaries.
- Per-file routing reasons.

This reduces debugging time and gives agents better final summaries.

### 8. Separate Raw AI Work From Durable Runtime Data

Raw GPT Pro prompts, outputs, and research packets may be useful but should not all become source code.

Better pattern:

- Store private raw AI work outside Git.
- Commit normalized runtime data only when the app or tests need it.
- Keep generated fixtures in a separate private artifact store if they are large or frequently regenerated.
- Document restore and upload commands.

### 9. Tune Agent Workflow To The Project Stage

In early development, fast shared iteration can be the right default:

- Commit and push meaningful changes quickly.
- Deploy runtime-affecting changes when the workflow expects deployed state to stay current.
- Run quick practical checks before sharing.
- Do not leave useful work only on one laptop.

For later stages, the same pattern can be tightened with pull requests, approval gates, longer tests, or environment promotion.

## The Post Should Emphasize

- This was a real flaw found through use, not a theoretical architecture preference.
- The improved workflow is more about software engineering discipline than AI novelty.
- AI agents amplify the structure of the repo and deployment system.
- Modular cloud ownership makes agent work safer because it gives the agent smaller, named surfaces to change.
- Path-routed checks and deploys are only safe after ownership is documented and tested.

## Avoid

- Naming private projects or domains.
- Mentioning account IDs, emails, tokens, secrets, or proprietary product details.
- Turning this into an AWS tutorial.
- Claiming AI agents replace engineers.
- Presenting the first workflow as a failure. It was a useful first version with a scaling flaw.
