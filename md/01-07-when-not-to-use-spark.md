## When NOT to use Spark

Spark earns its complexity only at scale. Before you reach for it, know when it's the wrong tool:

- **Your data fits comfortably in pandas.** Below roughly ten gigabytes on a modern laptop, pandas is faster, simpler, and carries no cluster overhead.
- **You need single-row, sub-millisecond lookups.** Spark is throughput-optimized, not latency-optimized. Reach for Postgres, DynamoDB, or Redis instead.
- **You need OLTP semantics.** Spark is built for analytical, read-heavy, write-batched workloads. It is not a transactional database.
- **You only need a scheduled script.** Sometimes a `cron` job plus a plain Python script is the right answer.

The honest rule: Spark earns its complexity only when a single machine actually stops being enough — when scale or fault tolerance genuinely demand it.
