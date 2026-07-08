# AWS Profiles, SSO, And Codex

Use AWS profiles and SSO so Codex can run AWS commands without plaintext credentials in prompts or committed files.

## Mental Model

GitHub is the code source of truth.
AWS is the data and artifact source of truth.

Use AWS CLI profiles for account and role selection.
Use short-lived SSO credentials instead of long-lived static keys where possible.
Do not paste AWS secrets, access keys, session tokens, or secret values into prompts or docs.

## Back Up AWS Files

Before changing AWS CLI config, back up the current files:

```bash
mkdir -p ~/.aws/backups
cp ~/.aws/config ~/.aws/backups/config.$(date +%Y%m%d%H%M%S) 2>/dev/null || true
cp ~/.aws/credentials ~/.aws/backups/credentials.$(date +%Y%m%d%H%M%S) 2>/dev/null || true
```

## Configure A Shared SSO Session

Edit `~/.aws/config`:

```bash
nano ~/.aws/config
```

Use a shared SSO session block:

```ini
[sso-session main]
sso_start_url = https://YOUR-START-URL.awsapps.com/start
sso_region = us-east-1
sso_registration_scopes = sso:account:access
```

Replace the start URL and region with the organization-specific values.

## Configure Profiles

Use named profiles for each project or account:

```ini
[profile project-dev]
sso_session = main
sso_account_id = 123456789012
sso_role_name = AdministratorAccess
region = us-east-1
output = json
```

Use a default profile only when it is genuinely safe for everyday shell commands:

```ini
[default]
region = us-east-1
output = json
```

## Refresh SSO

Refresh an SSO profile:

```bash
aws sso login --profile project-dev
```

Verify identity:

```bash
aws sts get-caller-identity --profile project-dev
```

## Launch Codex With AWS Credentials

Prefer explicit AWS profile commands:

```bash
AWS_PROFILE=project-dev codex
```

Or export a profile for the current shell:

```bash
export AWS_PROFILE=project-dev
codex
```

Inside Codex, keep AWS commands explicit when possible:

```bash
AWS_PROFILE=project-dev aws sts get-caller-identity
```

## AWS Core Plugin Notes

The AWS Core Codex plugin provides AWS-focused skills and MCP tooling.
It may bundle Codex lifecycle hooks.
If global Codex hooks are disabled, the plugin can remain installed while bundled hooks do not run.

Review plugin hooks with:

```text
/hooks
```

## Subagent Notes

When delegating AWS work to subagents or FirstMate crewmates, pass the intended profile and region explicitly.
Avoid relying on ambient credentials when the task could affect production resources.

## Optional AWS Output Benchmark

For large AWS responses, prefer compact output:

```bash
aws service operation --profile project-dev --region us-east-1 --query '...' --output json
```

Use tool-specific wrappers such as `aws-toon-output` when available for token-efficient AWS inspection.
