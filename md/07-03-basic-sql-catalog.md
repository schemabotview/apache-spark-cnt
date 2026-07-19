## Basic SQL & the catalog API

The query surface is standard ANSI: `SELECT`, `WHERE`, `GROUP BY`, `HAVING`, `ORDER BY`, `LIMIT`. Note that Spark SQL **does** support `HAVING`, even though there's no `.having()` method on DataFrames (module 06 filtered groups with `.filter()` after `.agg()`).

Alongside queries, `spark.catalog` lets you inspect the metastore programmatically — for discovery and cleanup:

- **`listDatabases()`** / **`listTables()`** / **`listColumns(table)`** — enumerate what exists.
- **`tableExists(name)`** — a boolean guard.
- **`dropTempView(name)`** / **`dropGlobalTempView(name)`** — clean up views.

Queries read the data; the catalog reads the *metadata about* the data.
