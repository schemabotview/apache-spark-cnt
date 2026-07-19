## `partitionBy` — the partition-pruning win

`df.write.partitionBy("status")` writes one **subdirectory per distinct value** of the column:

```text
transactions_by_status/
    status=APPROVED/  part-00000.parquet
    status=DECLINED/  part-00000.parquet
    status=REVERSED/  part-00000.parquet
```

A query that filters on that column then reads **only the relevant subdirectories** — Spark never opens the rest. That's **partition pruning**, and it's the single biggest write-time decision for downstream read speed.

The one rule: pick a **low-cardinality** column — a few dozen distinct values at most. Partitioning by `customer_id` would create millions of tiny directories and wreck performance.
