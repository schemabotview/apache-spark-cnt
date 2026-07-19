## Setup & creating a `ps.DataFrame`

Import `pyspark.pandas as ps`, then reach for one of three everyday entry points:

- **From a Python dict** — the same constructor signature as pandas. Good for tiny fixtures.
- **From files** — `ps.read_csv`, `ps.read_parquet` go through Spark's readers under the hood.
- **From an existing Spark DataFrame** — `sdf.pandas_api()` wraps it as a `ps.DataFrame`. This is the **cheap, no-data-movement** path you'll use most.

The reverse from a real pandas frame — `ps.from_pandas(pdf)` — exists too, but use it **only for fixtures**: it serializes the pandas frame and re-reads it through Spark. In practice you start from a Spark DataFrame and call `pandas_api()`.
