# OpenClaw Adapter

Use this adapter when OpenClaw should expose Obsiwiki as a slash-command workflow.

## Entry Point

Recommended top-level command:

```text
/obsiwiki
```

Suggested subcommands:

```text
/obsiwiki ingest <url-or-source>
/obsiwiki capture
/obsiwiki capture <target-page>
/obsiwiki capture synthesis
/obsiwiki query <question>
/obsiwiki lint
/obsiwiki review
/obsiwiki review <range>
/obsiwiki weekly
/obsiwiki schedule
/obsiwiki help
```

Do not install `adapters/` into the local OpenClaw command directory. Read adapters from the repository as reference notes.

## Command Semantics

### `/obsiwiki ingest <url-or-source>`

Use when the user gives a URL, article, paper, transcript, meeting note, or raw note.

Expected behavior:

- analyze the source first without writing files
- consult `System/Schema/purpose.md` when present and state a value assessment
- propose `raw/`, `assets/raw/<source-slug>/`, `wiki/sources/<source-slug>.md`, concept/entity/synthesis, map, index, and log changes
- preserve source-relevant images, figures, diagrams, screenshots, PDFs, and other attachments under `assets/raw/<source-slug>/`; embed or link local copies from the raw/source note, preserve captions/alt text/source URLs when available, and record skipped assets with reasons
- use `wiki/review.md` when an issue needs human judgment
- write after confirmation unless the user requested automatic execution

### `/obsiwiki capture`

Use when a conversation produced reusable knowledge.

Expected behavior:

- extract conclusions, decisions, definitions, tradeoffs, and workflows
- avoid saving full chat transcripts by default
- consult `System/Schema/purpose.md` when present and state why the conclusion is worth preserving
- suggest the best target page
- use `wiki/review.md` when a valuable conclusion still needs human judgment
- write only after confirmation unless the user requested automatic execution

### `/obsiwiki query <question>`

Use when the user asks a question that should be answered from the vault.

Expected behavior:

- start with `wiki/index.md`
- follow relevant `wiki/maps/`
- read only the necessary formal pages
- suggest a `synthesis` update if the answer is broadly reusable

### `/obsiwiki lint`

Use for vault health checks.

Expected behavior:

- flag orphan pages
- flag formal pages not covered by maps or index
- flag source pages without formal links
- flag duplicate or near-duplicate topics
- flag missing `last_updated`, missing sources, or missing `Related Links`
- flag raw-source attachment placement issues
- flag wiki pages that should promote reusable assets from `raw/` into `assets/wiki/`
- flag stale or missing value guidance and support pages such as `System/Schema/purpose.md`, `wiki/overview.md`, or `wiki/review.md` when expected by the active schema
- flag graph health issues: clusters without maps, source clusters without concepts, concepts without sources, stale syntheses, overloaded maps, duplicate clusters, and bridge candidates
- add or propose `wiki/review.md` items when graph health issues require interpretation
- exclude `System/` from ordinary content lint
- run only lightweight configuration checks for `System/`, such as required schema files and broken internal links within schema docs

### `/obsiwiki review`

Use when the user wants a read-only review of recent additions, recent updates, or this week's knowledge base changes.

Expected behavior:

- default to the last 7 days when no range is provided
- support ranges such as `yesterday`, `this week`, `this month`, `last 14 days`, `since 2026-04-01`, and `2026-04-01 to 2026-04-15`
- read `wiki/log.md` first, page frontmatter `last_updated` second, and file modification time only as a fallback
- summarize the time range, new sources and pages, notable updates, topic clusters, open organization questions, open review items, overview drift, and suggested next `ingest`, `capture`, or `lint` actions
- propose `wiki/overview.md` updates when the review changes the compressed picture of the knowledge base
- propose `wiki/review.md` items for unresolved duplicate, source, stale synthesis, unclear value, or graph health questions
- do not write to the vault by default

### `/obsiwiki review <range>`

Same as `/obsiwiki review`, but use the provided time range.

### `/obsiwiki weekly`

Generate this week's knowledge base report as a `review` with the `this week` range. Do not save the report unless the user asks to switch to `capture` or confirms a `wiki/syntheses/` update.

### `/obsiwiki schedule`

Guide the user through creating a cron task or recurring automation for recent-update review and periodic lint.

Expected behavior:

- ask whether the user wants recurring review and lint
- let the user choose the cadence and time; default to Monday 09:00 in the user's locale
- ask whether review and lint should run as one combined job or separate jobs
- confirm the target vault path, output destination, and whether the scheduled job may write changes
- keep the scheduled job read-only by default
- show the final schedule and maintenance prompt before creating or modifying the task

## Rules

- Commands route into the shared `ingest / capture / query / lint / review` workflows.
- Directory semantics and page contracts come from the installed Obsiwiki schema by default.
- If the target vault contains `System/Schema/`, treat that vault-local schema as the source of truth.
- Do not maintain private OpenClaw rules that conflict with the active Obsiwiki schema.
- Prefer updating existing pages over creating duplicates.
- Consult `System/Schema/purpose.md` before promoting material into durable wiki content when present.

## Update Behavior

When updating the installed Obsiwiki skill, refresh the skill files first. Then, if the target vault has `System/Schema/`, compare the latest default schema, value guidance, and support pages with the vault-local files.

- Do not overwrite vault-local schema during a skill update.
- Report schema/workflow differences separately from value-guidance additions such as `System/Schema/purpose.md` and support-page additions such as `wiki/overview.md` and `wiki/review.md`.
- If `System/Schema/purpose.md` is missing, ask whether to initialize one from the starter template or draft one for this vault. Do not create it silently.
- If `wiki/overview.md` or `wiki/review.md` is missing, scan `wiki/index.md`, `wiki/maps/`, `wiki/log.md`, and recent formal page updates, then ask whether to initialize those support pages before depending on them. Draft `wiki/overview.md` as a current-state summary and `wiki/review.md` as a backlog with any duplicate-topic, missing-source, stale-synthesis, unclear-value, or graph-health issues found during the scan.
- If new workflow behavior exists only in the installed defaults, such as two-stage `ingest`, scheduled maintenance, or graph-health lint, ask the user whether to merge it into the vault-local schema before using it as active vault behavior.
