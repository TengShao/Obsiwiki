# Hermes Adapter

Use this adapter when Hermes or another non-Codex agent should maintain the vault with the same Obsiwiki rules.

## Context To Load

Point Hermes at these files:

- `SKILL.md`
- `references/schema.md`
- `references/page-types.md`
- `references/lint-checklist.md`
- `starter-vault/System/Schema/vault-schema.md`
- `starter-vault/System/Schema/workflows.md`
- `starter-vault/System/Schema/page-contracts.md`

Use the installed Obsiwiki schema as the default. If the target vault contains `System/Schema/`, prefer that vault-local schema over the installed starter files.

## Operating Rules

- Treat the vault as an agent-maintained LLM Wiki, not a folder of isolated notes.
- Use `System/Schema/` as the vault-local source of truth when present.
- Use two-stage draft-first behavior for `ingest`: source analysis first, proposed wiki changes second.
- Use suggest-and-confirm behavior for `capture`.
- Start `query` from `wiki/index.md` and relevant `wiki/maps/`.
- Keep `review` and weekly reports read-only by default; use the last 7 days unless the user specifies a range, and use `this week` for weekly reports.
- Consult `purpose.md` before promoting material into durable wiki content when present.
- Use `wiki/overview.md` as the compressed knowledge base state.
- Use `wiki/review.md` for uncertain value, duplicate, source, stale synthesis, or graph health decisions.
- For scheduled maintenance, ask whether the user wants recurring review and lint, let them choose the cadence, default to Monday 09:00 in their locale, and keep the job read-only unless they confirm writes.
- Use `wiki/maps/` as the main anti-orphan mechanism.
- Do not save full chat transcripts by default.
- Prefer updating existing pages over creating duplicates.

## Recommended Prompts

```text
Use Obsiwiki to ingest this source: https://example.com/article
Use Obsiwiki to capture reusable conclusions from this conversation.
Use Obsiwiki to answer this from the vault: ...
Use Obsiwiki to lint this vault.
Use Obsiwiki to review recent additions to this vault.
Use Obsiwiki to generate this week's knowledge base report.
Use Obsiwiki to set up scheduled maintenance for this vault.
```

## Prompt Template

```text
Use the Obsiwiki workflow.

Read:
- SKILL.md
- references/schema.md
- references/page-types.md
- references/lint-checklist.md
- System/Schema/ if present, otherwise the installed Obsiwiki schema

Maintain this vault through five workflows:
- ingest: analyze source first, then propose raw/source/page/map/index/log changes
- capture: extract reusable conclusions, ask before writing, update target pages
- query: answer from wiki/index.md, maps, concepts, entities, sources, syntheses
- lint: find orphan pages, missing sources, duplicates, stale pages, asset issues, and graph health issues
- review: summarize recent additions, weekly changes, topic clusters, open review items, overview drift, and next actions without writing
- scheduled maintenance: guide creation of a recurring review/lint job; default to Monday 09:00 unless the user chooses another schedule

Keep raw sources in raw/, binary assets in assets/, durable knowledge in wiki/, value judgment in purpose.md, review backlog in wiki/review.md, and workflow rules in the active Obsiwiki schema.
```
