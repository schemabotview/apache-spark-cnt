## Actions — eager, trigger a Spark job

Each operation here *fires a job* — the plan built up by the transformations runs all the way through Catalyst, materializes, and returns either pandas-side data or a write side effect. Five families:

- **Display / preview** — `head`, `tail`, `print(df)`.
- **Driver materialization** — `to_pandas`, `.values`, `.iloc[i]` — these pull to the driver; treat them like `collect()`.
- **Frame-wide reductions** — `count`, `sum`, `mean`, `describe`, `len(df)`.
- **Writes** — `to_csv`, `to_parquet`, `to_table`.
- **Iteration** — `iterrows`, `itertuples` — **avoid on large data** (they pull every row to the driver).

The dangerous ones are the driver-materialization and iteration families — the same driver-bottleneck lesson from module 09, wearing a pandas face.
