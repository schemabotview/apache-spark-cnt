## Narrow vs wide transformations

This is the single most important distinction for reasoning about Spark performance.

- **Narrow transformation** — each output partition depends on **one** input partition. **No data crosses the network.** Examples: `map`, `filter`, `flatMap`, `mapPartitions`, `union`. Narrow transformations pipeline together inside a single stage.
- **Wide transformation** — each output partition depends on **many** input partitions, so data must be **shuffled** across the network. Examples: `reduceByKey`, `groupByKey`, `join`, `distinct`, `repartition`. Each wide transformation creates a new **stage boundary**.

Wide transformations are the expensive ones — a shuffle writes to disk, moves data over the network, and starts a new stage. That is the rule of thumb you carry through the rest of the curriculum: **narrow is cheap, wide is where the cost lives.**
