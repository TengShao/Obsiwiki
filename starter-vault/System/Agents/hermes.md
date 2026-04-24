---
title: Hermes Adapter
type: agent-adapter
tags: []
last_updated: 2026-04-24
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

Recommended behavior:

- Use draft-first behavior for `ingest`.
- Use suggest-and-confirm behavior for `capture`.
- Start `query` from `wiki/index.md` and relevant `wiki/maps/`.
- Use `wiki/maps/` as the main anti-orphan mechanism.
- Keep `System/Schema/` as the source of truth.
