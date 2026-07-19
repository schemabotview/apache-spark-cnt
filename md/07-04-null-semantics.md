## Null semantics — `col == null` always returns null

ANSI three-valued logic applies, with one DataFrame-specific trap that trips everyone at least once.

The trap: **`col == None` always evaluates to `null`, never `true`.** So `df.filter(col("category") == None)` returns **zero rows** — a filter only passes rows where the predicate is `true`, and `category == null` is `null` for *every* row. The same holds in SQL: `WHERE col = NULL` never matches.

The correct tools:

- **`col.isNull()` / `col.isNotNull()`** — the DataFrame way; **`col IS NULL` / `col IS NOT NULL`** — the SQL way.
- **`col <=> null` / `col.eqNullSafe(...)`** — null-safe equality; returns `true` when both sides are null.

And `col1 == col2` returns `null` whenever either side is null, so those rows drop out of a filter unless you use `eqNullSafe`.
