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
- Use draft-first behavior for `ingest`.
- Use suggest-and-confirm behavior for `capture`.
- Start `query` from `wiki/index.md` and relevant `wiki/maps/`.
- Use `wiki/maps/` as the main anti-orphan mechanism.
- Do not save full chat transcripts by default.
- Prefer updating existing pages over creating duplicates.

## Recommended Prompts

```text
Use Obsiwiki to ingest this source: https://example.com/article
Use Obsiwiki to capture reusable conclusions from this conversation.
Use Obsiwiki to answer this from the vault: ...
Use Obsiwiki to lint this vault.
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

Maintain this vault through four workflows:
- ingest: save raw sources, create source digests, update maps/index/log
- capture: extract reusable conclusions, ask before writing, update target pages
- query: answer from wiki/index.md, maps, concepts, entities, sources, syntheses
- lint: find orphan pages, missing sources, duplicates, stale pages, and asset issues

Keep raw sources in raw/, binary assets in assets/, durable knowledge in wiki/, and workflow rules in the active Obsiwiki schema.
```
