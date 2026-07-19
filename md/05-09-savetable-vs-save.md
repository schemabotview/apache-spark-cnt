## `saveAsTable` vs `save` — catalog vs filesystem

Two ways to write, with a crucial difference in *discoverability*:

- **`save(path)`** — writes files, full stop. You get a directory of files the catalog knows nothing about; you reload with `spark.read.format(...).load(path)`.
- **`saveAsTable(name)`** — writes files *and* registers the table in `spark.catalog`. You can then `spark.table(name)` or query it in SQL with `SELECT * FROM name`.

Use **`saveAsTable`** for anything someone else might query — SQL-style discoverability. Use **`save`** for file-only outputs: intermediate staging, or exports for downstream tools that don't read the catalog. (And recall from the last section: `bucketBy` needs `saveAsTable`.)
