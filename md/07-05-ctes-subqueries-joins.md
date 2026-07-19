## CTEs, subqueries & SQL joins

Three ways Spark SQL composes queries:

- **CTEs (`WITH ... AS (...)`)** — name intermediate results so a complex query reads **top-to-bottom instead of inside-out**. Full ANSI `WITH` chains are supported.
- **Subqueries** — correlated and uncorrelated, in `WHERE`, `FROM`, and `SELECT`: a **scalar** subquery (single value), an **`IN`** subquery (set membership), and an **`EXISTS`** subquery (correlated — the outer row passes if the inner query returns any row).
- **Joins** — all six types, same semantics as the DataFrame `how` argument, just SQL syntax: `INNER`, `LEFT` / `RIGHT` / `FULL OUTER`, and `LEFT SEMI` / `LEFT ANTI`.

Everything you learned about DataFrame joins and set logic applies unchanged — only the syntax differs.
