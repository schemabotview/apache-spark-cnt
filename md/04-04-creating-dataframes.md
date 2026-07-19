## Creating DataFrames

Five common ways — the first three are workhorses for tests, the last two are everyday:

- **`spark.createDataFrame(data, schema)`** — a Python list plus an explicit schema (preferred).
- **`spark.createDataFrame(data)`** — a Python list with schema inference.
- **`spark.range(n)`** — a fast integer-sequence DataFrame for testing.
- **`spark.createDataFrame(pandas_df)`** — round-trip from pandas.
- **`spark.read.format(...).load(path)`** — from real files (the next module).

In real pipelines the last one dominates; the in-memory constructors are mostly for tests and demos.
