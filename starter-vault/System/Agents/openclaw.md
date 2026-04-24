---
title: OpenClaw Adapter
type: agent-adapter
tags: []
last_updated: 2026-04-24
aliases: []
---

# OpenClaw Adapter

Recommended slash entry:

- `/wiki`

Suggested subcommands:

- `/wiki ingest <url>`
- `/wiki capture`
- `/wiki capture [[target page]]`
- `/wiki capture synthesis`
- `/wiki query <question>`
- `/wiki lint`
- `/wiki help`

Rules:

- Commands route into the shared `ingest / capture / query / lint` workflows.
- Directory semantics and page contracts come from `System/Schema/`.
- Do not maintain private OpenClaw rules that conflict with the schema.
