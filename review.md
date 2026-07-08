# Review

This file defines the review process agents should use after each slice of progress.
A slice of progress is a small coherent unit of work, such as one bug fix, one feature increment, one refactor step, or one deployable change.
Agents should not wait until a large feature is complete before reviewing.

The review process has three gates:

1. A1: Integration Gate.
2. A2: Correctness Gate.
3. A3: Release Readiness Gate.

Each gate must either pass or produce concrete follow-up work.
If a gate fails, the agent should investigate the failure, fix the issue, and rerun the relevant gate.
Agents should not ignore, hand-wave, or defer failed review checks unless the user explicitly accepts that risk.

## A1: Integration Gate

A1 verifies that the current branch is up to date with the latest main branch and that integration conflicts are resolved.
The goal is to review and test the change against the current codebase, not stale code.

Required steps:

1. Fetch the latest remote state.
2. Sync the working branch with `main`.
3. Resolve any conflicts.
4. Re-run the relevant checks after conflict resolution.

Prefer rebase for local agent branches unless the repo policy requires merge-based synchronization.
Some repositories prefer merge commits or protected branch workflows, so the universal rule is to sync with `main`, not always to rebase.

Typical rebase workflow:

```bash
git fetch origin
git rebase origin/main
```

Typical merge workflow:

```bash
git fetch origin
git merge origin/main
```

If conflicts occur, the agent should resolve them carefully, inspect the affected files, and rerun the relevant A2 checks.
Conflict resolution can introduce new bugs, even when the original change was correct.

## A2: Correctness Gate

A2 verifies that the change is logically correct in a fresh and adversarial review context.
The reviewer should not simply trust the implementing agent's assumptions.
The reviewer should inspect the diff, look for edge cases, and try to find ways the change could fail.

The adversarial review should look for:

- Incorrect logic.
- Missing edge cases.
- Broken user flows.
- Security or privacy issues.
- Bad abstractions.
- Incorrect assumptions about external systems.
- Missing tests.
- Regressions in adjacent behavior.
- Poor error handling.

After the adversarial review, run the repo's local verification checks.
Each project should replace these placeholders with its actual commands.

Type checks:

```bash
# Example only.
npm run typecheck
```

Smoke tests:

```bash
# Example only.
npm run smoke
```

Unit tests:

```bash
# Example only.
npm test
```

Lint checks:

```bash
# Example only.
npm run lint
```

The agent should record what was run and whether each command passed.
If a command fails, the agent should inspect the failure, fix the cause when practical, and rerun the command.
If the failure is unrelated to the current change, the agent should still report it clearly and explain why it appears unrelated.

## A3: Release Readiness Gate

A3 verifies that the change is ready for broader review, CI, and deployment.
This is the full code review stage.
It should be broader than style and should consider whether the change fits the system.

The full code review should check:

- Architecture fit.
- API compatibility.
- Data model and migration safety.
- Deployment risk.
- Rollback risk.
- Observability.
- Error handling.
- Security and privacy impact.
- Documentation updates.
- User-facing behavior.
- UI quality when applicable.
- Test coverage.

Run CI/CD checks through the repo's normal workflow.
For GitHub projects, this usually means checking pull request checks or GitHub Actions results.

Example commands:

```bash
# Example only.
gh pr checks
gh run list --limit 10
```

Run end-to-end tests when the change affects user flows, integrations, infrastructure, authentication, billing, permissions, data movement, or deployment behavior.

Example command:

```bash
# Example only.
npm run test:e2e
```

If CI/CD or E2E checks fail, the agent should investigate, fix the issue when practical, and rerun the failed checks.
The agent should not promote the change until A3 passes or the user explicitly accepts the remaining risk.

## Failure Handling

Failure handling is part of the review process.
A failed gate is not the end of the process.
It is a signal to debug, fix, and verify again.

When any gate fails, the agent should:

1. Identify the failing command, review finding, conflict, or CI/CD check.
2. Inspect the relevant code, logs, test output, or diff.
3. Determine whether the failure is caused by the current change.
4. Fix the issue when practical.
5. Rerun the smallest relevant check that proves the issue is fixed.
6. Rerun broader checks if the fix touches shared behavior or high-risk code.
7. Record what failed, what changed, and what passed afterward.

If the agent cannot fix the issue, it should explain the blocker clearly.
The explanation should include the failing command or check, the observed error, the likely cause, and the next action needed.

## Review Evidence

Every completed review should leave a short evidence trail.
The evidence can live in a pull request comment, task update, final agent message, or project tracking file.

Recommended format:

```md
## Review Evidence

A1 Integration Gate:
- Synced with main: yes
- Method: rebase
- Conflicts: none

A2 Correctness Gate:
- Adversarial review: completed
- Type checks: passed
- Smoke tests: passed
- Unit tests: passed
- Lint: passed

A3 Release Readiness Gate:
- Full code review: completed
- CI/CD checks: passed
- E2E tests: passed

Notes:
- No known remaining review risks.
```

## Repo Customization

Each repo should customize this file with its real commands.
Replace example commands with the actual typecheck, smoke test, unit test, lint, CI/CD, and E2E commands for the project.
Keep the A1, A2, and A3 gate structure unless the repo has a stronger review process.
