---
title: Claude Code Adapter
type: agent-adapter
tags: []
last_updated: 2026-04-24
aliases: []
---

# Claude Code Adapter

Use Obsiwiki to maintain this vault as an agent-friendly LLM Wiki.

## Recommended Setup

Create a `CLAUDE.md` file at the vault root that points Claude Code to:

- `System/Schema/vault-schema.md`
- `System/Schema/workflows.md`
- `System/Schema/page-contracts.md`
- `System/Agents/claude-code.md`

## Typical Requests

- `Use Obsiwiki to ingest this source: https://...`
- `Use Obsiwiki to capture reusable conclusions from this conversation.`
- `Use Obsiwiki to answer this from the vault: ...`
- `Use Obsiwiki to lint this vault.`

## Claude Code Rules

- Follow the installed Obsiwiki schema by default.
- If this vault contains `System/Schema/`, treat it as the vault-local source of truth.
- Use draft-first behavior for `ingest`.
- Use suggest-and-confirm behavior for `capture`.
- Do not save full conversation transcripts by default.
- Start `query` from `wiki/index.md` and relevant `wiki/maps/`.
- Prefer updating existing wiki pages over creating duplicates.
- Update `wiki/log.md` after meaningful ingest, capture, lint, and synthesis changes.
