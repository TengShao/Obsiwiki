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
- `/obsiwiki help`

Rules:

- Commands route into the shared `ingest / capture / query / lint / review` workflows.
- Keep `review` and weekly reports read-only by default; use the last 7 days unless the user specifies a range, and use `this week` for `/obsiwiki weekly`.
- Directory semantics and page contracts come from the installed Obsiwiki schema by default.
- If this vault contains `System/Schema/`, treat it as the vault-local source of truth.
- Do not maintain private OpenClaw rules that conflict with the active Obsiwiki schema.
