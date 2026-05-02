---
title: OpenClaw Adapter
type: agent-adapter
tags: []
last_updated: 2026-04-28
aliases: []
---

# OpenClaw Adapter

Recommended slash entry:

- `/obsiwiki`

Suggested subcommands:

- `/obsiwiki ingest <url-or-source>`
- `/obsiwiki capture`
- `/obsiwiki capture <target-page>`
- `/obsiwiki capture synthesis`
- `/obsiwiki query <question>`
- `/obsiwiki lint`
- `/obsiwiki review`
- `/obsiwiki review <range>`
- `/obsiwiki weekly`
- `/obsiwiki schedule`
- `/obsiwiki help`

Rules:

- Commands route into the shared `ingest / capture / query / lint / review` workflows.
- Use two-stage draft-first behavior for `ingest`: source analysis first, proposed wiki changes second.
- Keep `review` and weekly reports read-only by default; use the last 7 days unless the user specifies a range, and use `this week` for `/obsiwiki weekly`.
- Consult `System/Schema/purpose.md` before promoting material into durable wiki content when present.
- Use `wiki/overview.md` as the compressed knowledge base state.
- Use `wiki/review.md` for uncertain value, duplicate, source, stale synthesis, or graph health decisions.
- For `/obsiwiki schedule`, ask whether the user wants recurring review and lint, let them choose the cadence, default to Monday 09:00 in their locale, and keep the job read-only unless they confirm writes.
- Directory semantics and page contracts come from the installed Obsiwiki schema by default.
- If this vault contains `System/Schema/`, treat it as the vault-local source of truth.
- During skill updates, compare the latest installed default schema, value guidance, and support pages with this vault before using new workflow behavior. Do not overwrite `System/Schema/`; report schema/workflow differences separately from value-guidance additions such as `System/Schema/purpose.md` and support-page additions such as `wiki/overview.md` and `wiki/review.md`; if `System/Schema/purpose.md` is missing, ask whether to initialize one from the starter template or draft one for this vault; if `wiki/overview.md` or `wiki/review.md` is missing, ask whether to initialize those support pages before depending on them; then ask before merging.
- Do not maintain private OpenClaw rules that conflict with the active Obsiwiki schema.
