# Codex Hooks

Codex lifecycle hooks can run commands around tool calls, permission requests, compaction, and session events.
Hooks are useful for enforcement, but they should be reviewed before trust.

## Default Preference

Disable Codex lifecycle hooks globally unless a project explicitly needs them.
This keeps plugins installed and available while preventing bundled hooks from running automatically.

Add this to `~/.codex/config.toml`:

```toml
[features]
hooks = false
```

Verify the setting:

```bash
codex features list | rg hooks
```

Expected result:

```text
hooks                                stable             false
plugin_hooks                         removed            false
```

## Reviewing Hooks

If a plugin asks to trust hooks at startup, review the hooks with:

```text
/hooks
```

Do not trust hooks blindly.
For example, the AWS plugin may bundle `PreToolUse` hooks that run a secret-safety check before shell or AWS tool calls.

## Git Hooks Are Different

Codex hooks are not Git hooks.
Git hooks live under `.git/hooks` or a configured `core.hooksPath`.
Codex hooks live in Codex config layers, `hooks.json`, or installed plugins.
