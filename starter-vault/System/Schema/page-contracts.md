---
title: Page Contracts
type: schema
tags: []
last_updated: 2026-04-24
aliases: []
---

# Page Contracts

## `wiki/index.md`

- Acts as the top-level navigation entry.
- Every high-value formal wiki page should have an index record or be reachable through a map.
- Use this format: `[[Page Name]] - one sentence description`.

## `wiki/log.md`

- Append-only maintenance log.
- Record at least:
  - `ingest`
  - `capture`
  - `lint`
  - major `query -> synthesis` updates

## `wiki/concepts/`

- Complete minimal frontmatter.
- First paragraph gives a one-sentence definition.
- Link at least one upstream source.
- Include `## 关联连接` with at least one related page or map.

## `wiki/entities/`

- Complete minimal frontmatter.
- Explain what the entity is and why it matters.
- Link at least one source or upstream reference.
- Be listed in at least one map or `wiki/index.md`.

## `wiki/sources/`

- One-to-one with a raw source.
- Include summary, key points, value, and related topics.
- Link to at least one `concept`, `entity`, or `map`.

## `wiki/syntheses/`

- Store long-lived reusable analysis across multiple pages, sources, or conversations.
- Link multiple formal pages.
- Be listed in at least one map.

## `wiki/maps/`

- Topic maps / MOCs.
- Main anti-orphan mechanism.
- Useful structure:
  - topic description
  - core pages
  - related sources
  - related opinions, projects, or work pages
  - adjacent maps
