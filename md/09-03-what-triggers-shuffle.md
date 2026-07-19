## What triggers a shuffle

A shuffle happens whenever Spark must **redistribute data across partitions** — and it's the most expensive thing Spark does. Every shuffle is three steps:

1. **Shuffle write** — each task writes intermediate output to local disk.
2. **Network transfer** — data moves to the target executors.
3. **Shuffle read** — receiving tasks read and deserialize.

Operations that **always** shuffle (wide): `groupBy`, `join`, `distinct`, `orderBy`, `repartition`, `dropDuplicates`, and `union` with dedup.

Operations that **never** shuffle (narrow): `filter`, `select`, `withColumn`, `coalesce` (when reducing), `map`, `flatMap`.

The practical skill is spotting shuffles in a plan: each one appears as an **`Exchange`** node in `.explain()`, and each marks a stage boundary. Fewer shuffles = fewer stages = faster jobs.
