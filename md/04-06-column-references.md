## Column references — the four ways

PySpark accepts four styles for naming a column. They're *mostly* interchangeable, but each has a place:

- **`"col_name"`** (a string) — for a simple `select`, `groupBy`, or `sort`. Can't apply functions or build expressions.
- **`col("col_name")`** — for building expressions and passing to `when`, `coalesce`, `F.sum`. The safest default.
- **`df["col_name"]`** — for disambiguating columns from two DataFrames in a join. Verbose for normal use.
- **`expr("amount * 0.02")`** — for pasting a SQL-style snippet. Easy to typo; parsed at runtime.

Rule of thumb: a **string** when the API just needs a name, **`col()`** when building an expression, **`df["c"]`** when joining, and **`expr()`** sparingly.
