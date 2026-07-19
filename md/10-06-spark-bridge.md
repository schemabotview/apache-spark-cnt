## The Spark bridge — `to_spark()` and `pandas_api()`

Because a `ps.DataFrame` is just a wrapper, flipping between it and a Spark DataFrame is **cheap** — neither direction moves data; they only swap whether the index is exposed pandas-style or hidden Spark-style:

- **Spark DF → Pandas-on-Spark**: `sdf.pandas_api()` — cheap (wrap).
- **Pandas-on-Spark → Spark DF**: `psdf.to_spark()` — cheap (unwrap).

The **expensive** directions are the ones that end at real pandas — `sdf.toPandas()` and `psdf.to_pandas()` both do a **driver collect**. The rule from module 04 repeats: the methods with the lowercase singular **`pandas`** in the name move data to the driver — treat them like `collect()`. Everyday work stays in the cheap wrap / unwrap zone and only crosses to real pandas for a small, final result.
