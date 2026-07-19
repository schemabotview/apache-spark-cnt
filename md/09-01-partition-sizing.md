## Partition sizing — the unit of parallelism

A partition is processed by exactly one task on one executor core — so the partition count *is* your parallelism, and getting it wrong hurts in two directions:

- **Too few partitions** — cores sit idle, one slow task stalls the whole stage, and a huge partition risks an out-of-memory error.
- **Too many partitions** — task-scheduling overhead dominates; tiny tasks finish in milliseconds while the scheduler spends longer setting them up than the work takes.

Rules of thumb:

- **Production / cluster** — target **128–256 MB per partition**.
- **Local mode** — **2–4× the number of CPU cores** is plenty.
- **After a shuffle** — the count is set by `spark.sql.shuffle.partitions` (default 200 — tuned next).

Inspect the current count with `df.rdd.getNumPartitions()`, and the row distribution with `spark_partition_id()`.
