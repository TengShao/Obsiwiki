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
/obsiwiki help
```

## Command Semantics

### `/obsiwiki ingest <url-or-source>`

Use when the user gives a URL, article, paper, transcript, meeting note, or raw note.

Expected behavior:

- preserve source material in `raw/`
- store attachments in `assets/raw/<source-slug>/`
- create or update `wiki/sources/<source-slug>.md`
- update related `concept`, `entity`, or `synthesis` pages where useful
- attach new or updated pages to at least one `wiki/maps/` page
- update `wiki/index.md` and `wiki/log.md`

### `/obsiwiki capture`

Use when a conversation produced reusable knowledge.

Expected behavior:

- extract conclusions, decisions, definitions, tradeoffs, and workflows
- avoid saving full chat transcripts by default
- suggest the best target page
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
- flag attachment placement issues

## Rules

- Commands route into the shared `ingest / capture / query / lint` workflows.
- Directory semantics and page contracts come from the installed Obsiwiki schema by default.
- If the target vault contains `System/Schema/`, treat that vault-local schema as the source of truth.
- Do not maintain private OpenClaw rules that conflict with the active Obsiwiki schema.
- Prefer updating existing pages over creating duplicates.
