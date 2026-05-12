# Schema

Use these write targets:

- raw text sources: `raw/`
- binary attachments: `assets/`
- durable knowledge: `wiki/`
- value judgment guidance: `System/Schema/purpose.md`
- current knowledge state: `wiki/overview.md`
- uncertain issues and human decisions: `wiki/review.md`
- disposable generated review/report artifacts: `exports/reviews/`
- vault marker: `System/obsiwiki.toml`
- rules and contracts: `System/Schema/`

## Vault Location

Agents should resolve the target vault before reading or writing:

1. Use an explicit path from the user.
2. Use `OBSIWIKI_VAULT` if the environment exposes it.
3. Use `~/.config/obsiwiki/vaults.toml` if it is readable.
4. Walk upward from the current working directory until `System/obsiwiki.toml` is found.
5. As a legacy fallback, walk upward until both `wiki/index.md` and `System/Schema/` are found.

After the user confirms a vault path, record it in `~/.config/obsiwiki/vaults.toml` when that location is writable. Keep vault-local paths relative inside `System/obsiwiki.toml`.

If multiple vaults match or no vault matches, ask the user to choose. The installed Obsiwiki skill directory is not itself the user's vault.

Key rules:

- Do not write binary files into `raw/`.
- During ingest, source-relevant images, figures, diagrams, screenshots, PDFs, and other attachments should be stored under `assets/raw/<source-slug>/` and embedded or linked from the raw/source note as local vault assets.
- Preserve captions, alt text, and source URLs for ingested assets when available; record skipped or unavailable assets with a short reason.
- Do not treat `System/Agents/` as the source of truth.
- Consult `System/Schema/purpose.md` before promoting material into durable wiki content.
- Use `wiki/maps/` to absorb pages into the graph before over-optimizing body links.
- Use `wiki/review.md` when an issue needs human judgment instead of forced automatic cleanup.
- Keep generated HTML dashboards and other disposable reports under `exports/reviews/`; do not treat them as durable wiki knowledge or sources of truth.
