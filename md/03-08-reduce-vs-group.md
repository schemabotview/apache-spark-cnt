## reduceByKey vs groupByKey — the exam classic

Both group values by key, and both are **wide** transformations, so both shuffle. The difference is *how much* they shuffle — and it's the single most-tested RDD gotcha.

- **`reduceByKey(f)`** combines values **locally on each partition first** — a map-side combine — then shuffles only the partial results. Far less data crosses the network.
- **`groupByKey()`** ships **every value** across the network to the reducer, then you combine. On a large dataset this floods the shuffle and can blow up executor memory.

The rule: **whenever you're going to aggregate, prefer `reduceByKey`** (or `aggregateByKey` / `combineByKey`) over `groupByKey` followed by a reduce. Reach for `groupByKey` only when you genuinely need every value in a group as a collection, not an aggregate.

The same lesson carries to DataFrames: `groupBy().agg()` does the map-side combine for you — one more reason to prefer the DataFrame API.
