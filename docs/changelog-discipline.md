# AI Changelog Discipline

AI agents need a memory trail.

Code history tells what changed. A changelog explains why the agent changed it, what it verified, and what future sessions need to know.

## What Belongs In An AI Changelog

Include entries for:

- Architecture changes.
- Deployment changes.
- AWS resource creation or cleanup.
- Auth changes.
- Data model changes.
- Meaningful product decisions.
- Known limitations or follow-up tasks.

Do not turn the changelog into noise. Small typo fixes and purely mechanical formatting changes usually do not need detailed entries.

## Useful Entry Format

```md
## YYYY-MM-DD

- Changed X so that Y.
- Verified with Z.
- Follow-up: A still needs B.
```

## Why It Works

The changelog is not bureaucracy. It is working memory for future AI sessions.

A future agent can read it and understand:

- What was already tried.
- Which AWS resources exist.
- Which choices were deliberate.
- Which parts are temporary.
- Which risks are known.

That context reduces repeated investigation and prevents accidental reversions.
