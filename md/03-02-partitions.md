## Partitions — the unit of parallelism

An RDD is split into **partitions** — chunks of data that can be processed independently. Each partition is processed by exactly one task, on one executor core. That single fact sets the degree of parallelism:

- **Number of partitions = degree of parallelism** for that RDD.
- **More partitions than cores** → some tasks queue, but every core stays busy.
- **Fewer partitions than cores** → cores sit idle.

Two defaults to know. After a shuffle, the output partition count defaults to `spark.sql.shuffle.partitions` — **200** unless you change it. For an RDD created with `parallelize`, the default is `sc.defaultParallelism`, usually the total number of cores available.

Right-sizing partitions — enough to use every core, not so many that task-startup overhead dominates — is a theme that returns in the performance-tuning module.
