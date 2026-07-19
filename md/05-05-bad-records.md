## Bad-record handling — the `mode` option

When an input row doesn't conform to the schema, Spark needs a policy — set it with the `mode` option:

- **`PERMISSIVE`** (default) — set the bad row's failed columns to `null` and keep the row. With `columnNameOfCorruptRecord` set, the raw bad string is stashed in that column.
- **`DROPMALFORMED`** — silently drop the bad row.
- **`FAILFAST`** — throw an exception on the first bad row.

Separately, set **`badRecordsPath`** to write rejected rows to their own location for offline inspection — very useful in production, where you want the pipeline to keep running but still capture what it skipped.
