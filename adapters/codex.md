# Codex Adapter

Use this adapter when Codex should maintain an Obsidian vault with Obsiwiki.

Codex normally loads Obsiwiki through the installed skill named `obsiwiki`. Use `$obsiwiki` when you want to invoke the skill explicitly.

## Context To Load

Codex should use:

- `SKILL.md`
- `references/schema.md`
- `references/page-types.md`
- `references/lint-checklist.md`
- `references/review-dashboard.md`
- `templates/review-dashboard.html`
- `starter-vault/System/Schema/vault-schema.md`
- `starter-vault/System/Schema/workflows.md`
- `starter-vault/System/Schema/page-contracts.md`
- `starter-vault/System/Schema/purpose.md`
- `starter-vault/wiki/overview.md`
- `starter-vault/wiki/review.md`

Use the installed Obsiwiki schema as the default. If the target vault contains `System/Schema/`, prefer that vault-local schema over the installed starter files.

Do not install `adapters/` into the local Codex skill directory. Read adapters from the repository as reference notes.

## Vault Location

Before any workflow, resolve the target vault in this order:

1. Path explicitly provided by the user.
2. `OBSIWIKI_VAULT`, when available.
3. `~/.config/obsiwiki/vaults.toml`, when readable.
4. Current working directory or an ancestor containing `System/obsiwiki.toml`.
5. Legacy fallback: current working directory or an ancestor containing both `wiki/index.md` and `System/Schema/`.

After the user confirms a vault path, record it in the user-level registry when possible. Keep `System/obsiwiki.toml` portable and do not write absolute paths into it.

## Update Behavior

When updating the installed Obsiwiki skill, refresh the skill files first. Then, if the target vault has `System/Schema/`, compare the latest default schema, value guidance, and support pages with the vault-local files.

- Do not overwrite vault-local schema during a skill update.
- Report schema/workflow differences separately from value-guidance additions such as `System/Schema/purpose.md` and support-page additions such as `wiki/overview.md` and `wiki/review.md`.
- If `System/Schema/purpose.md` is missing, ask whether to initialize one from the starter template or draft one for this vault. Do not create it silently.
- If `wiki/overview.md` or `wiki/review.md` is missing, scan `wiki/index.md`, `wiki/maps/`, `wiki/log.md`, and recent formal page updates, then ask whether to initialize those support pages before depending on them. Draft `wiki/overview.md` as a current-state summary and `wiki/review.md` as a backlog with any duplicate-topic, missing-source, stale-synthesis, unclear-value, or graph-health issues found during the scan.
- If new workflow behavior exists only in the installed defaults, such as two-stage `ingest`, scheduled maintenance, or graph-health lint, ask the user whether to merge it into the vault-local schema before using it as active vault behavior.

## Operating Rules

- Treat the installed `SKILL.md` as the main Codex skill body.
- Treat `System/Schema/` as the vault-local source of truth when present.
- Use two-stage draft-first behavior for `ingest`: source analysis first, proposed wiki changes second.
- During `ingest`, preserve source-relevant images, figures, diagrams, screenshots, PDFs, and other attachments under `assets/raw/<source-slug>/`; embed or link local copies from the raw/source note, preserve captions/alt text/source URLs when available, and record skipped assets with reasons.
- Use suggest-and-confirm behavior for `capture`.
- Start `query` from `wiki/index.md` and relevant `wiki/maps/`.
- Keep `review` and weekly reports read-only by default; use the last 7 days unless the user specifies a range, and use `this week` for weekly reports.
- Use `templates/review-dashboard.html` as the agent-side webpage generation template when the user asks for an HTML dashboard, interactive report, visual review, or exported report file; write generated webpages under `exports/reviews/`.
- Consult `System/Schema/purpose.md` before promoting material into durable wiki content when present.
- Use `wiki/overview.md` as the compressed knowledge base state.
- Use `wiki/review.md` for uncertain value, duplicate, source, stale synthesis, or graph health decisions.
- For scheduled maintenance, ask whether the user wants recurring review and lint, let them choose the cadence, default to Monday 09:00 in their locale, and keep the job read-only unless they confirm writes.
- Do not save full chat transcripts by default.
- Prefer updating existing pages over creating duplicates.
