---
title: Codex Adapter
type: agent-adapter
tags: []
last_updated: 2026-04-24
aliases: []
---

# Codex Adapter

Use the skill explicitly:

```text
$obsiwiki
```

Typical requests:

- `用 $obsiwiki 处理这个链接：https://...`
- `用 $obsiwiki 把刚才这段讨论沉淀进知识库`
- `用 $obsiwiki 查询 A2A 和 MCP 的关系`
- `用 $obsiwiki lint 我的 vault`

Codex rules:

- Follow `System/Schema/`.
- Use draft-first behavior for `ingest`.
- Use suggest-and-confirm behavior for `capture`.
- Do not save full conversation transcripts by default.
- Prefer updating existing wiki pages over creating duplicates.
