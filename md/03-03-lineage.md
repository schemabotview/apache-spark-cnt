## Lineage — how Spark recovers from failure

Spark does **not** replicate RDD data the way HDFS does. Instead it remembers the *recipe* — the sequence of transformations that produced each RDD. That recipe is the **lineage graph**.

If a partition is lost — an executor crashes, a machine dies — Spark replays just the relevant slice of the lineage to rebuild that one partition. Cheap when the lineage is short; expensive when it's long. Resilience comes from the pure functions plus lineage, not from copying the data.

This is also *why caching matters*: without `.cache()`, every action re-runs the full lineage from the source. Reuse an RDD in two actions and you compute it twice. Caching (next-but-one section) breaks that by materializing the RDD after the first action.
