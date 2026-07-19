## `repartition` vs `coalesce` vs `partitionBy` — three different things

Three operations all have "partition" in the name and get routinely confused — they do **completely different jobs**:

- **`df.repartition(n)`** — redistribute rows across `n` partitions **in memory**; a **full shuffle**. Use it to *increase* the count or rebalance.
- **`df.coalesce(n)`** — merge existing partitions down to `n` **without redistributing**; **no shuffle** (narrow). Only *decreases* the count.
- **`df.write.partitionBy("col")`** — at **write time**, create one on-disk **subdirectory** per distinct value (from module 05). No in-memory effect — it changes file layout only.

The key split is **runtime vs. write-time**: `repartition` / `coalesce` change how rows sit **in memory** for the next stage; `partitionBy` changes how they sit **on disk** for downstream readers.

One more form: **`repartition(n, col)`** hash-partitions in memory by a column, so all rows with the same key land together — do it before a join on that column to **eliminate the join-time shuffle**.
