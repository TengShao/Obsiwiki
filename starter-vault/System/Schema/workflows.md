---
title: Workflows
type: schema
tags: []
last_updated: 2026-04-24
aliases: []
---

# Workflows

This vault supports four primary workflows: `ingest`, `capture`, `query`, and `lint`.

## Ingest

Use when the user provides a URL, article, PDF, screenshot pack, transcript, meeting note, or raw note.

Default mode: draft first, then confirm.

Steps:

1. Read title, author, publication date, URL, and body where available.
2. Save attachments under `assets/raw/<source-slug>/`.
3. Save the original source or source-like note under `raw/`.
4. Create or update a one-to-one digest under `wiki/sources/`.
5. Search for related `concepts`, `entities`, and `syntheses`.
6. Prefer updating existing pages over creating duplicates.
7. Attach new or updated pages to relevant `maps`.
8. Show the user a concise draft of proposed changes.
9. Write changes after confirmation unless the user asked for automatic execution.
10. Update `wiki/index.md`.
11. Append an entry to `wiki/log.md`.

## Capture

Use when a conversation produced reusable conclusions, decisions, definitions, tradeoffs, or workflows.

Default mode: suggest target, ask for confirmation, then update directly.

Steps:

1. Identify reusable knowledge.
2. Present a structured draft:
   - core conclusion
   - suggested target page
   - possible conflicts with existing knowledge
   - whether a `synthesis` page is warranted
3. Ask for confirmation before writing.
4. Prefer updating the target page directly.
5. Create a `wiki/syntheses/` page only when the result spans multiple pages or the target is ambiguous.
6. Update relevant maps, `wiki/index.md`, and `wiki/log.md`.

Do not save full chat transcripts by default.

## Query

Use when the user asks a question that should be answered from the existing wiki.

Steps:

1. Start with `wiki/index.md` and related `wiki/maps/`.
2. Read relevant `concepts`, `entities`, `sources`, and `syntheses`.
3. Answer from formal wiki pages when possible.
4. If the answer becomes broadly reusable, suggest updating or creating a `synthesis`.

## Lint

Use for vault health checks and structural cleanup.

Check for:

- completely isolated pages
- formal wiki pages not covered by maps or index
- `sources/` pages that do not point to a concept, entity, or map
- duplicate or near-duplicate topics
- missing sources, missing `last_updated`, or missing `关联连接`
- high-value raw/source pages that should be promoted into formal wiki pages
- maps that only list raw/source pages without durable concepts or syntheses
- raw pages with attachments that are not stored under `assets/raw/`
- wiki pages that depend on raw-only assets that should be promoted to `assets/wiki/`
