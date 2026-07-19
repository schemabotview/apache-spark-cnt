## From your code to distributed work

When you write `df.filter(...).groupBy(...).count()`, here is what happens behind the scenes:

1. The driver builds a **logical plan** from your DataFrame chain.
2. The **Catalyst** optimizer rewrites it — pushing filters down, reordering joins, and dropping unused columns.
3. The optimized plan becomes a **physical plan**, then a **DAG** of stages.
4. Each stage splits into **tasks** — one task per partition.
5. Tasks ship to the executors and run in parallel across their CPU cores.

Three vocabulary words to lock in now, because they show up constantly on the exam:

- **Job** — what one action triggers. One `.count()` call is one job.
- **Stage** — a slice of work between shuffles. A new shuffle starts a new stage.
- **Task** — the smallest unit of work: one partition, processed by one executor core.
