## Converting between the three types

Conversion is the central skill the exam tests — each direction has a specific method *and a cost*:

- **Spark DF → Pandas-on-Spark**: `sdf.pandas_api()` — **cheap** (just a wrapper).
- **Pandas-on-Spark → Spark DF**: `psdf.to_spark()` — **cheap** (unwraps the wrapper).
- **pandas → Pandas-on-Spark**: `ps.from_pandas(pdf)` — Spark reads and distributes.
- **pandas → Spark DF**: `spark.createDataFrame(pdf)` — Spark reads and distributes.
- **Spark DF → pandas**: `sdf.toPandas()` — **expensive** (all rows to the driver).
- **Pandas-on-Spark → pandas**: `psdf.to_pandas()` — **expensive** (all rows to the driver).

Rule of thumb: the methods with the lowercase word **`pandas`** in them (not the `pandas_api` wrapper) pull data to the driver. Treat them like `collect()` — only on small or already-aggregated results.
