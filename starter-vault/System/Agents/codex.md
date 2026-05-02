---
title: Codex Adapter
type: agent-adapter
tags: []
last_updated: 2026-04-28
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
- `Use Obsiwiki to review recent additions to this vault.`
- `Use Obsiwiki to generate this week's knowledge base report.`

Codex rules:

- Follow the installed Obsiwiki schema by default.
- If this vault contains `System/Schema/`, treat it as the vault-local source of truth.
- Use two-stage draft-first behavior for `ingest`: source analysis first, proposed wiki changes second.
- Use suggest-and-confirm behavior for `capture`.
- Keep `review` and weekly reports read-only by default; use the last 7 days unless the user specifies a range, and use `this week` for weekly reports.
- Consult `purpose.md` before promoting material into durable wiki content when present.
- Use `wiki/overview.md` as the compressed knowledge base state.
- Use `wiki/review.md` for uncertain value, duplicate, source, stale synthesis, or graph health decisions.
- Do not save full conversation transcripts by default.
- Prefer updating existing wiki pages over creating duplicates.
