## Batch vs Structured Streaming — the mental model

Spark batch and Spark streaming **share the same execution engine** — the partition → task → executor model is identical. What changes is the *lifecycle around it*.

In one line: **batch** is a *bounded* stream of partitions with a known end — the DAG plans the whole job upfront, runs once, terminates. **Streaming** is an *unbounded* sequence of **micro-batches**, where each micro-batch is *itself* a bounded batch — the same execution, run repeatedly.

Four things diverge:

- **DAG lifecycle** — one execution vs. the same logical DAG re-run per micro-batch.
- **State store** — none vs. RocksDB-backed state on executors for stateful ops.
- **Output modes** — write-and-exit vs. `append` / `update` / `complete` (there's running state to emit).
- **Fault tolerance** — rerun the job vs. checkpoint + offset tracking → resume exactly where it stopped.

Everything else — partitioning, shuffles, Catalyst, executor memory — is the **same code path**. The unifying insight: *streaming is batch executed on small, frequent slices of an unbounded source.*
