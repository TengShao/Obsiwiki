---
title: Workflows
type: schema
tags: []
last_updated: 2026-04-28
aliases: []
---

# Workflows

This vault supports five primary workflows: `ingest`, `capture`, `query`, `lint`, and `review`.

Scheduled maintenance is orchestration around `review` and `lint`, not a separate content workflow.

## Ingest

Use when the user provides a URL, article, PDF, screenshot pack, transcript, meeting note, or raw note.

Default mode: draft first, then confirm.

Stage 1: source analysis. Do not write files in this stage.

1. Read title, author, publication date, URL, and body where available.
2. Consult `System/Schema/purpose.md` when present.
3. Identify the core thesis, reusable claims, important entities, concepts, and examples.
4. Search for related `concepts`, `entities`, `syntheses`, maps, and review items.
5. Identify possible duplicates, conflicts, missing sources, or synthesis candidates.
6. State a short value assessment.

Stage 2: proposed wiki changes.

1. Propose where to save attachments under `assets/raw/<source-slug>/`.
2. Propose where to save the original source or source-like note under `raw/`.
3. Propose the one-to-one digest under `wiki/sources/`.
4. Propose updates to existing `concepts`, `entities`, or `syntheses` before creating duplicates.
5. Propose map, `wiki/index.md`, and `wiki/log.md` updates.
6. If an issue needs human judgment, propose a structured item in `wiki/review.md` instead of forcing a final decision.
7. Write changes after confirmation unless the user asked for automatic execution.

## Capture

Use when a conversation produced reusable conclusions, decisions, definitions, tradeoffs, or workflows.

Default mode: suggest target, ask for confirmation, then update directly.

Steps:

1. Identify reusable knowledge.
2. Present a structured draft:
   - core conclusion
   - suggested target page
   - value assessment against `System/Schema/purpose.md` when present
   - possible conflicts with existing knowledge
   - whether a `synthesis` page is warranted
   - whether a `wiki/review.md` item is needed
3. Ask for confirmation before writing.
4. Prefer updating the target page directly.
5. Create a `wiki/syntheses/` page only when the result spans multiple pages or the target is ambiguous.
6. Update relevant maps, `wiki/index.md`, and `wiki/log.md`.
7. Add a review item when the conclusion is valuable but needs later human judgment.

Do not save full chat transcripts by default.

## Query

Use when the user asks a question that should be answered from the existing wiki.

Steps:

1. Start with `wiki/index.md` and related `wiki/maps/`.
2. Read relevant `concepts`, `entities`, `sources`, and `syntheses`.
3. Answer from formal wiki pages when possible.
4. If the answer becomes broadly reusable, suggest updating or creating a `synthesis`.

## Review

Use when the user wants a read-only review of recent additions, recent updates, or this week's knowledge base changes.

Default range: the last 7 days unless the user specifies a range.

Supported ranges include `yesterday`, `this week`, `this month`, `last 14 days`, `since 2026-04-01`, and `2026-04-01 to 2026-04-15`.

Treat a weekly knowledge base report as a `review` with the `this week` range.

Steps:

1. Resolve the requested time range.
2. Read `wiki/log.md` first to identify recent ingest, capture, lint, and synthesis activity.
3. Use page frontmatter `last_updated` to find additions or updates missing from the log.
4. Use file modification time only as a fallback, and state that it is a fallback when it affects the answer.
5. Read only the relevant `wiki/` pages needed to explain what changed.
6. Answer with:
   - time range
   - new sources and pages
   - notable updates
   - topic clusters
   - open organization questions
   - open review items
   - overview drift
   - suggested next `ingest`, `capture`, or `lint` actions
7. When useful, propose updates to `wiki/overview.md`.
8. When uncertain issues need follow-up, propose structured items for `wiki/review.md`.

Do not write to the vault by default. If the user wants to save a weekly report or durable summary, switch to `capture` or propose a `wiki/syntheses/` update and ask for confirmation before writing.

## Scheduled Maintenance

Use when the user asks for automation, recurring maintenance, scheduled review, scheduled lint, weekly review, or cron setup.

Default schedule: every Monday at 09:00 in the user's locale.

Steps:

1. Ask whether the user wants to create scheduled maintenance for recent-update review and periodic lint.
2. Let the user choose the cadence and time. Offer the default schedule when the user has no preference.
3. Ask whether `review` and `lint` should run as one combined job or as separate jobs.
4. Confirm the target vault path and output destination.
5. Confirm whether the scheduled task may write follow-up changes. Default to read-only.
6. Use the host agent's native scheduler when available; otherwise guide the user through cron or the environment's preferred automation mechanism.
7. Show the final schedule and maintenance prompt before creating or modifying the scheduled task.

Default maintenance prompt:

```text
/obsiwiki review recent additions to this vault and lint this vault. Keep the run read-only by default. Summarize recent additions, notable updates, open review items, overview drift, lint issues, graph health issues, and suggested next actions. Propose any durable writes for user confirmation.
```

## Lint

Use for vault health checks and structural cleanup.

Check for:

- exclude `System/` files from ordinary content lint
- lightweight `System/` configuration health: key schema files are readable, `System/Agents/` follows `System/Schema/`, and vault-local schema customizations are preserved
- completely isolated pages
- formal wiki pages not covered by maps or index
- `sources/` pages that do not point to a concept, entity, or map
- duplicate or near-duplicate topics
- missing sources, missing `last_updated`, or missing `关联连接`
- high-value raw/source pages that should be promoted into formal wiki pages
- maps that only list raw/source pages without durable concepts or syntheses
- raw pages with attachments that are not stored under `assets/raw/`
- wiki pages that depend on raw-only assets that should be promoted to `assets/wiki/`
- missing or stale value guidance or support pages such as `System/Schema/purpose.md`, `wiki/overview.md`, or `wiki/review.md` when those files are expected by the active schema
- graph health issues:
  - cluster without map: several related pages are not covered by a coherent map
  - source cluster without concept: multiple sources point to the same durable idea but no concept page exists
  - concept without sources: a concept contains durable claims but has no upstream source or explicit user judgment
  - stale synthesis: a synthesis depends on pages that changed recently but the synthesis was not reviewed
  - overloaded map: a map is only a link dump without grouping, descriptions, or organizing judgment
  - duplicate cluster: titles, aliases, sources, or related links suggest overlapping pages
  - bridge candidate: two maps share enough pages or themes that a synthesis or cross-link may be useful

If a graph health issue requires interpretation, add or propose a `wiki/review.md` item rather than treating it as an automatic failure.
