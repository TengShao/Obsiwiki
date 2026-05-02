---
title: Page Contracts
type: schema
tags: []
last_updated: 2026-04-28
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
- `review` and weekly reports are read-only by default and should not append log entries unless the user confirms a follow-up `capture` or `synthesis` write.

## `purpose.md`

- Vault-level value judgment guide for agents.
- Lists the vault mission, active themes, value criteria, low-value content, and agent decision rule.
- Agents should consult it before promoting material into `wiki/`.
- `purpose.md` can be multi-topic. It should not force the vault into a single theme.
- When value is unclear, agents should suggest or create a review item instead of silently promoting the material.

## `wiki/overview.md`

- Compressed state of the current knowledge base.
- Summarizes current shape, mature areas, emerging areas, knowledge gaps, important maps, and recent direction.
- Updated during `review`, weekly reports, or explicit overview refreshes.
- Not required after every small ingest.

## `wiki/review.md`

- Structured backlog for uncertain issues, human decisions, and later agent follow-up.
- Agents may add open items with status, type, related pages, evidence, suggested action, and whether user decision is required.
- Agents should not close items that require user judgment unless the user confirms the decision.
- Resolved items should keep a short resolution note.

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
