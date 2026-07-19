## Lazy evaluation, in depth

The rule is simple; here is the reference to keep handy when reading other people's code.

Transformations are **lazy** — they append to the plan and return another DataFrame: `select`, `filter`, `withColumn`, `groupBy`, `join`, `orderBy`. Actions are **eager** — they trigger DAG execution and return a value, a file, or printed output: `show`, `count`, `collect`, `take`, `first`, `write`.

Because evaluation is lazy, Spark gets room to be clever before any data moves:

- **Push filters early** — a filter written at the *end* of the chain may execute right next to the data scan, so less data flows through the pipeline.
- **Prune projections** — columns you never reference are dropped before reading.
- **Fuse operations** — several narrow transformations collapse into one tight loop (whole-stage codegen, three sections down).
- **Skip everything** — define transformations and never call an action, and no work runs at all.

Every one of these wins is possible *only because* Spark waited.
