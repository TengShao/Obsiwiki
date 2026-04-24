# Schema

Use these write targets:

- raw text sources: `raw/`
- binary attachments: `assets/`
- durable knowledge: `wiki/`
- rules and contracts: `System/Schema/`

Key rules:

- Do not write binary files into `raw/`.
- Do not treat `System/Agents/` as the source of truth.
- Use `wiki/maps/` to absorb pages into the graph before over-optimizing body links.
