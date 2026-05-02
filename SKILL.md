---
name: obsiwiki
description: Maintain an Obsidian vault as an agent-agnostic LLM Wiki. Use Obsiwiki when Codex or another agent needs to ingest external links or raw notes into raw/wiki, capture valuable discussion outcomes into formal notes, answer questions from wiki pages, review recent additions or weekly knowledge base changes, maintain index and map pages, manage assets for raw or wiki content, or lint the vault for orphans, duplicate topics, missing links, missing sources, and stale pages.
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
4. `purpose.md`, when present, tells agents how to judge what is worth preserving.
5. `System/Schema/`, when present in the target vault, is the vault-local source of truth for workflows and page contracts.
6. `Work/Projects/Opinions/Journal/Archive/` store applied outputs and personal writing when the vault uses those folders.

## Schema Precedence

Use the installed Obsiwiki `references/` and `starter-vault/System/Schema/` as the default schema.

If the target vault contains `System/Schema/`, treat that vault-local schema as the source of truth. Use the installed schema only for comparison, update suggestions, and migrations. Do not silently overwrite vault-local schema customizations.

Agent adapters translate the same schema for each agent. They must not fork directory semantics, page contracts, or workflow behavior.

## Workflow Selection

Choose one of five workflows:

- `ingest`: when the user gives a URL, article, paper, transcript, meeting note, or raw note
- `capture`: when a discussion has produced a stable conclusion worth storing
- `query`: when the user asks a question that should be answered from existing wiki pages
- `lint`: when the user wants a vault health check or structural cleanup
- `review`: when the user wants a read-only review of recent additions, recent updates, or this week's knowledge base changes

## Ingest Rules

- Use a two-stage flow: source analysis first, proposed wiki changes second.
- During source analysis, do not write files. Identify the source thesis, reusable claims, entities, concepts, related pages, possible duplicates, conflicts, and synthesis candidates.
- Consult `purpose.md` when present and include a short value assessment.
- During proposed changes, name the `raw/`, `assets/raw/<source-slug>/`, `wiki/sources/`, concept/entity/synthesis, map, index, and log updates.
- Prefer updating an existing `concept`, `entity`, or `synthesis` page over creating duplicates.
- If an issue needs human judgment, propose or add a structured item in `wiki/review.md` instead of forcing a decision.
- Write changes after confirmation unless the user explicitly asks for automatic execution.

## Capture Rules

- Do not store full chat transcripts by default.
- Extract only reusable conclusions, decisions, definitions, tradeoffs, or workflows.
- Consult `purpose.md` when present and include a short value assessment.
- Suggest the best target page.
- Default to updating the target page directly after confirmation.
- Only create a `synthesis` page when the outcome spans multiple pages or the target is still ambiguous.
- Use `wiki/review.md` when a valuable conclusion still needs human judgment.

## Query Rules

- Start with `wiki/index.md` and related `wiki/maps/`.
- Read only the pages needed for the answer.
- If a cross-topic answer becomes broadly reusable, suggest updating or creating a `synthesis`.

## Review Rules

- Default to the last 7 days when the user does not specify a range.
- Accept natural-language ranges such as `yesterday`, `this week`, `this month`, `last 14 days`, `since 2026-04-01`, or `2026-04-01 to 2026-04-15`.
- Treat a weekly knowledge base report as a `review` with the `this week` range.
- Determine recent additions from `wiki/log.md` first, page frontmatter `last_updated` second, and file modification time only as a fallback.
- Structure the answer around time range, new sources and pages, notable updates, topic clusters, open organization questions, and suggested next `ingest`, `capture`, or `lint` actions.
- Propose `wiki/overview.md` updates when the review changes the compressed picture of the knowledge base.
- Propose or add `wiki/review.md` items for unresolved organization, source, duplicate, stale synthesis, or graph health questions.
- Keep `review` read-only by default. If the user wants to save a weekly report or durable summary, switch to `capture` or propose a `wiki/syntheses/` update and ask for confirmation before writing.

## Lint Rules

- Do not treat `System/` files as ordinary wiki content. Exclude `System/Schema/` and `System/Agents/` from orphan, map coverage, source, `last_updated`, and `关联连接` checks.
- For `System/`, run only lightweight configuration checks: verify key schema files are readable, confirm `System/Agents/` does not redefine directory semantics, and compare vault-local schema with installed defaults only when useful.
- Flag wiki pages not covered by `maps/` or `index`.
- Flag `sources/` pages without links to a concept, entity, or map.
- Flag duplicate or near-duplicate topics.
- Flag missing `last_updated`, missing sources, or missing `关联连接`.
- Flag raw pages whose attachments are not stored in `assets/raw/`.
- Flag wiki pages that still depend on raw-only assets that should be promoted to `assets/wiki/`.
- Flag graph health issues: clusters without maps, source clusters without concepts, concepts without sources, stale syntheses, overloaded maps, duplicate clusters, and bridge candidates between maps.
- When a graph health issue requires interpretation, add or propose a `wiki/review.md` item rather than treating it as an automatic failure.

## Editing Style

- Preserve the user's naming style and existing language choices.
- Keep raw notes source-like and lightweight.
- Make wiki notes synthetic, linked, and reusable.
- Use maps to absorb otherwise isolated notes instead of forcing decorative body links.
