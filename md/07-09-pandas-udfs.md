## Pandas UDFs — vectorized via Apache Arrow

A **Pandas UDF** (or **vectorized UDF**) transfers data in **columnar Arrow batches** instead of row-by-row. Inside, you operate on a `pd.Series` (or a `pd.DataFrame` for grouped variants), so NumPy/pandas vectorization applies and Arrow removes most of the serialization overhead — dramatically faster than a plain Python UDF.

Three flavors worth knowing:

- **Series → Series** (the default) — a `pd.Series` in, a `pd.Series` out. For most row-wise transformations.
- **Iterator of Series** — an iterator in, an iterator out. Lets you **set up once** (load a model, open a DB connection) before processing the batches.
- **`groupBy().applyInPandas(f, schema)`** — a full `pd.DataFrame` per group. For custom aggregation that needs real pandas.

If you must leave the built-ins, reach for a Pandas UDF before a plain one.
