---
name: obsiwiki
description: Maintain an Obsidian vault as an agent-agnostic LLM Wiki. Use Obsiwiki when Codex or another agent needs to ingest external links or raw notes into raw/wiki, capture valuable discussion outcomes into formal notes, answer questions from wiki pages, maintain index and map pages, manage assets for raw or wiki content, or lint the vault for orphans, duplicate topics, missing links, missing sources, and stale pages.
---

# Obsiwiki

Treat the vault as a compiled knowledge system rather than a pile of isolated notes.

Read these references as needed:

- `references/schema.md` for directory semantics and write targets
- `references/page-types.md` for note routing and page expectations
- `references/lint-checklist.md` for maintenance checks

## Core Model

1. `raw/` stores original text sources.
2. `assets/` stores binary files, screenshots, PDFs, and reusable visuals.
3. `wiki/` stores durable knowledge pages.
4. `System/Schema/` is the source of truth for workflows and page contracts.
5. `Work/Projects/Opinions/Journal/Archive/` store applied outputs and personal writing when the vault uses those folders.

## Workflow Selection

Choose one of four workflows:

- `ingest`: when the user gives a URL, article, paper, transcript, meeting note, or raw note
- `capture`: when a discussion has produced a stable conclusion worth storing
- `query`: when the user asks a question that should be answered from existing wiki pages
- `lint`: when the user wants a vault health check or structural cleanup

## Ingest Rules

- Preserve the source in `raw/`.
- Store attachments in `assets/raw/<source-slug>/`.
- Create or update a matching page in `wiki/sources/`.
- Prefer updating an existing `concept`, `entity`, or `synthesis` page over creating duplicates.
- Attach the new or updated content to at least one relevant `map`.
- Before writing major changes, present a concise draft changes summary unless the user explicitly asks for automatic execution.

## Capture Rules

- Do not store full chat transcripts by default.
- Extract only reusable conclusions, decisions, definitions, tradeoffs, or workflows.
- Suggest the best target page.
- Default to updating the target page directly after confirmation.
- Only create a `synthesis` page when the outcome spans multiple pages or the target is still ambiguous.

## Query Rules

- Start with `wiki/index.md` and related `wiki/maps/`.
- Read only the pages needed for the answer.
- If a cross-topic answer becomes broadly reusable, suggest updating or creating a `synthesis`.

## Lint Rules

- Flag wiki pages not covered by `maps/` or `index`.
- Flag `sources/` pages without links to a concept, entity, or map.
- Flag duplicate or near-duplicate topics.
- Flag missing `last_updated`, missing sources, or missing `关联连接`.
- Flag raw pages whose attachments are not stored in `assets/raw/`.
- Flag wiki pages that still depend on raw-only assets that should be promoted to `assets/wiki/`.

## Editing Style

- Preserve the user's naming style and existing language choices.
- Keep raw notes source-like and lightweight.
- Make wiki notes synthetic, linked, and reusable.
- Use maps to absorb otherwise isolated notes instead of forcing decorative body links.
