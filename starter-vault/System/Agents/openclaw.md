---
title: OpenClaw Adapter
type: agent-adapter
tags: []
last_updated: 2026-04-24
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
- `/obsiwiki help`

Rules:

- Commands route into the shared `ingest / capture / query / lint` workflows.
- Directory semantics and page contracts come from the installed Obsiwiki schema by default.
- If this vault contains `System/Schema/`, treat it as the vault-local source of truth.
- Do not maintain private OpenClaw rules that conflict with the active Obsiwiki schema.
