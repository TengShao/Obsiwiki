---
title: Codex Adapter
type: agent-adapter
tags: []
last_updated: 2026-04-24
aliases: []
---

# Codex Adapter

Use the same canonical prompts as other agents. In Codex, `$obsiwiki` can also be used when you want to explicitly invoke the skill:

```text
$obsiwiki
```

Typical requests:

- `Use Obsiwiki to ingest this source: https://...`
- `Use Obsiwiki to capture reusable conclusions from this conversation.`
- `Use Obsiwiki to answer this from the vault: ...`
- `Use Obsiwiki to lint this vault.`

Codex rules:

- Follow the installed Obsiwiki schema by default.
- If this vault contains `System/Schema/`, treat it as the vault-local source of truth.
- Use draft-first behavior for `ingest`.
- Use suggest-and-confirm behavior for `capture`.
- Do not save full conversation transcripts by default.
- Prefer updating existing wiki pages over creating duplicates.
