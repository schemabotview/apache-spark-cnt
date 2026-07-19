## The Spark API hierarchy

Spark has three structured-data APIs, in increasing order of abstraction:

- **RDD** — the original. Functional, row-by-row. **No Catalyst optimization, no Tungsten codegen.**
- **DataFrame** — a distributed table with a schema. The everyday API.
- **Dataset[T]** — a typed DataFrame with compile-time type safety. **Scala / Java only.**

In PySpark you only ever get DataFrames. Internally, `DataFrame` is just an alias for `Dataset[Row]` — same engine, no static type parameter.

The practical fact to carry forward: **SQL, DataFrame, and (Scala) Dataset all compile through Catalyst and produce identical execution plans.** Only RDD bypasses Catalyst — which is exactly why it's slower for structured data.
