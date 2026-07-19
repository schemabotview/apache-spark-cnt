## Filtering groups, pivot & unpivot

Three group-shaping tools:

- **Filtering groups** — Spark has no `HAVING` keyword. Just chain `.filter()` *after* `.agg()` to filter on an aggregated column: `groupBy("c").agg(_sum("x").alias("t")).filter(col("t") > 100)`.
- **Pivot** — `pivot` rotates a column's distinct values into separate columns. **Always pass the value list explicitly** — `pivot("col", [...])` — to skip the discovery scan and lock the output column order.
- **Unpivot** — the inverse (columns → rows). Three forms: `stack(n, ...)` via `selectExpr` (works on all 3.x), and `df.unpivot(...)` / `df.melt(...)` (Spark 3.4+, familiar from pandas).

A wide reporting table (one column per category) becomes a long `(category, value)` table for charting or storage.
