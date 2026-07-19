## Temporary views — session, global, and managed tables

To query a DataFrame with SQL you first give it a name — a **view**. Three lifetimes:

- **`df.createOrReplaceTempView(name)`** — lives for **this SparkSession only**; address it as `<name>`.
- **`df.createOrReplaceGlobalTempView(name)`** — lives for the **whole JVM** (all sessions); address it as `global_temp.<name>`.
- **`df.write.saveAsTable(name)`** — **persisted** in the metastore (from module 05); address it as `<name>`.

This unlocks the **SQL ↔ DataFrame round-trip**: register a DataFrame as a view, query it with `spark.sql(...)`, take the result back as a DataFrame, keep chaining. Every hop goes through the same optimizer, so the plans stay merged.
