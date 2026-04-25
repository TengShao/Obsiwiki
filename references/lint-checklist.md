# Lint Checklist

Check these first:

- Is the file under `System/`? If yes, exclude it from ordinary content lint.
- For `System/`, only check configuration health: key schema files are readable, agent adapters follow `System/Schema/`, and vault-local schema customizations are not silently overwritten.
- Is the page completely isolated?
- Is it at least attached to one map or the index?
- Does it have a clear type and `last_updated`?
- Does it cite or link its upstream source?
- Does a `source` page point to at least one concept, entity, or map?
- Is a `synthesis` page linked from a map and multiple formal pages?
- Are raw attachments stored in `assets/raw/`?
- Should any raw-only asset be promoted to `assets/wiki/`?
