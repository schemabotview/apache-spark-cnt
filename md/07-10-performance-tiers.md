## Performance tiers — when to use each

A decision ladder — fastest to slowest — for "how should I express this?":

1. **Built-in `pyspark.sql.functions`** — string/date/array/conditional/numeric work. Tungsten codegen, no serialization. *Always try this first.*
2. **Higher-order functions** (`transform`, `filter`, `aggregate`) — element-wise array/map work; the lambda runs on the JVM.
3. **Pandas UDF — Series → Series** — NumPy/pandas/scikit-learn vectorized work; Arrow columnar batches.
4. **Pandas UDF — Iterator** — setup-once patterns (load a model, open a connection): one setup per batch, not per row.
5. **`applyInPandas`** — custom group-level logic that genuinely needs full pandas.
6. **Plain `@udf`** — simple custom logic on small data with no built-in alternative. Acceptable, but **avoid on hot paths** — 10–100× slower, with GC pressure on billions of rows.

The rule: climb this ladder only as far as you must, and rewrite hot `@udf`s downward.
