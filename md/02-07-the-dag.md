## The DAG — stages & shuffle boundaries

Once Catalyst hands over a physical plan, the **DAG scheduler** turns it into the units that actually run. A DAG — directed acyclic graph — is the plan drawn as a graph of steps that only ever flows forward.

The scheduler's one big decision is *where to cut the DAG into stages*, and the rule is precise: **a stage boundary is a shuffle.** Narrow transformations — `map`, `filter`, `select` — need only their own partition's data, so they chain together inside a single stage with no data movement. Wide transformations — `groupBy`, `join`, `distinct` — need data from *other* partitions, which forces a **shuffle** across the network, and that shuffle is exactly where one stage ends and the next begins.

Inside a stage, the work fans out into **tasks** — one task per partition, run on an executor core. So the hierarchy is: one action triggers a **job**; the job splits at shuffles into **stages**; each stage fans out into **tasks**.

This is why counting the shuffles in `.explain()` tells you how many stages a query has — and why removing a shuffle is one of the biggest wins in tuning.
