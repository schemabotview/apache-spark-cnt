## Driver-side bottlenecks — `collect`, `withColumn` in a loop

Some patterns look fast but stall the **driver or the planner**, not the executors.

**`collect()` pulls every row to the driver.** A DataFrame is distributed; `df.collect()` undoes that — every row travels back to a single Python list and **OOMs the driver** on large data. Always aggregate or `limit()` first: `groupBy(...).count().collect()` and `limit(100).collect()` are safe; a raw `big_df.collect()` is not. (`toPandas()` has the same trap.)

**`withColumn` in a loop is exponentially slow.** Each `withColumn` returns a *new* DataFrame whose plan contains the entire prior plan — 100 iterations build a 100-deep nested `Project`, and Catalyst re-analyzes the whole tree each time (analysis cost is super-linear in depth). A loop that takes minutes becomes milliseconds when you build the columns once and apply them in a **single `select`**. The same rule applies to any "iterate-and-extend" pattern.
