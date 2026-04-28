---
title: Hermes Adapter
type: agent-adapter
tags: []
last_updated: 2026-04-28
aliases: []
---

# Hermes Adapter

Point Hermes or another agent at:

- `SKILL.md`
- `references/schema.md`
- `references/page-types.md`
- `references/lint-checklist.md`
- `System/Schema/`

Typical requests:

- `Use Obsiwiki to ingest this source: https://...`
- `Use Obsiwiki to capture reusable conclusions from this conversation.`
- `Use Obsiwiki to answer this from the vault: ...`
- `Use Obsiwiki to lint this vault.`
- `Use Obsiwiki to review recent additions to this vault.`
- `Use Obsiwiki to generate this week's knowledge base report.`

Recommended behavior:

- Use draft-first behavior for `ingest`.
- Use suggest-and-confirm behavior for `capture`.
- Keep `review` and weekly reports read-only by default; use the last 7 days unless the user specifies a range, and use `this week` for weekly reports.
- Start `query` from `wiki/index.md` and relevant `wiki/maps/`.
- Use `wiki/maps/` as the main anti-orphan mechanism.
- Follow the installed Obsiwiki schema by default.
- If this vault contains `System/Schema/`, treat it as the vault-local source of truth.
