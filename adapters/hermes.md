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

If the target vault already contains `System/Schema/`, prefer the vault-local schema over the starter files.

## Operating Rules

- Treat the vault as an agent-maintained LLM Wiki, not a folder of isolated notes.
- Use `System/Schema/` as the source of truth.
- Use draft-first behavior for `ingest`.
- Use suggest-and-confirm behavior for `capture`.
- Start `query` from `wiki/index.md` and relevant `wiki/maps/`.
- Use `wiki/maps/` as the main anti-orphan mechanism.
- Do not save full chat transcripts by default.
- Prefer updating existing pages over creating duplicates.

## Recommended Commands

```text
/wiki ingest <url-or-source>
/wiki capture
/wiki capture <target-page>
/wiki query <question>
/wiki lint
```

## Prompt Template

```text
Use the Obsiwiki workflow.

Read:
- SKILL.md
- references/schema.md
- references/page-types.md
- references/lint-checklist.md
- System/Schema/

Maintain this vault through four workflows:
- ingest: save raw sources, create source digests, update maps/index/log
- capture: extract reusable conclusions, ask before writing, update target pages
- query: answer from wiki/index.md, maps, concepts, entities, sources, syntheses
- lint: find orphan pages, missing sources, duplicates, stale pages, and asset issues

Keep raw sources in raw/, binary assets in assets/, durable knowledge in wiki/, and workflow rules in System/Schema/.
```
