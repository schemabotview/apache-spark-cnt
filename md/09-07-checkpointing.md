## Checkpointing — truncating long lineage

Caching keeps the **lineage** — Spark can still recompute from the source if a cached partition is lost. **Checkpointing** goes further: it writes the DataFrame to reliable storage and **breaks the lineage**, so Spark forgets how the data was produced and just reads the saved files.

Reach for it when the *plan itself* becomes the problem:

- **Iterative algorithms** — ML training loops with 100+ iterations, where the lineage grows unbounded.
- **Long-running streaming** with stateful ops that accumulate lineage over time.
- **Any DAG longer than ~20 shuffle stages.**

The key contrast with `cache()`: **checkpointing is eager** — it executes immediately when called, not on the next action. Cache for *reuse*; checkpoint to *cut* a lineage that's grown too deep.
