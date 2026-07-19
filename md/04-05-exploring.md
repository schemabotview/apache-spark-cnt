## Exploring a DataFrame

The cheatsheet for *what does this DataFrame contain?*

- **`df.count()`** — number of rows (an action — triggers a job).
- **`df.columns`** — column names as a Python list.
- **`df.dtypes`** — `(name, type_string)` tuples.
- **`df.schema`** / **`df.printSchema()`** — the full `StructType`, raw or pretty-printed.
- **`df.describe()`** — count, mean, stddev, min, max for numeric columns; **`df.summary()`** adds quartiles.
- **`df.show(n, truncate=False)`** — print rows; pass `truncate=False` to see full content.

Most are cheap metadata reads; `count`, `describe`, `summary`, and `show` are actions that actually run a job.
