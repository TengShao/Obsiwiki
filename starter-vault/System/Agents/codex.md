---
title: Codex Adapter
type: agent-adapter
tags: []
last_updated: 2026-05-03
aliases: []
---

# Codex Adapter

In Codex, use `$obsiwiki` when you want to explicitly invoke the installed skill:

```text
$obsiwiki
```

Typical requests:

- `$obsiwiki ingest this source: https://...`
- `$obsiwiki capture reusable conclusions from this conversation.`
- `$obsiwiki answer this from the vault: ...`
- `$obsiwiki lint this vault.`
- `$obsiwiki review recent additions to this vault.`
- `$obsiwiki generate this week's knowledge base report.`
- `$obsiwiki set up scheduled maintenance for this vault.`

Codex rules:

- Follow the installed Obsiwiki schema by default.
- If this vault contains `System/Schema/`, treat it as the vault-local source of truth.
- Use two-stage draft-first behavior for `ingest`: source analysis first, proposed wiki changes second.
- During skill updates, compare the latest installed default schema, value guidance, and support pages with this vault before using new workflow behavior. Do not overwrite `System/Schema/`; report schema/workflow differences separately from value-guidance additions such as `System/Schema/purpose.md` and support-page additions such as `wiki/overview.md` and `wiki/review.md`; if `System/Schema/purpose.md` is missing, ask whether to initialize one from the starter template or draft one for this vault; if `wiki/overview.md` or `wiki/review.md` is missing, scan `wiki/index.md`, `wiki/maps/`, `wiki/log.md`, and recent formal page updates, then ask whether to initialize those support pages; draft `wiki/overview.md` as a current-state summary and `wiki/review.md` as a backlog with issues found during the scan; then ask before merging.
- Use suggest-and-confirm behavior for `capture`.
- Keep `review` and weekly reports read-only by default; use the last 7 days unless the user specifies a range, and use `this week` for weekly reports.
- Consult `System/Schema/purpose.md` before promoting material into durable wiki content when present.
- Use `wiki/overview.md` as the compressed knowledge base state.
- Use `wiki/review.md` for uncertain value, duplicate, source, stale synthesis, or graph health decisions.
- For scheduled maintenance, ask whether the user wants recurring review and lint, let them choose the cadence, default to Monday 09:00 in their locale, and keep the job read-only unless they confirm writes.
- Do not save full conversation transcripts by default.
- Prefer updating existing wiki pages over creating duplicates.
