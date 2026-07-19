## Output modes — append, update, complete

The output mode controls **which rows** Spark writes per micro-batch — and whether the **state store** turns on:

- **`append`** (default) — **stateless**; only new rows go out. For projections, filters, and windowed aggregations *with a watermark* (a closed window emits once).
- **`update`** — **stateful**; the state store holds running aggregates per key and emits only the keys that **changed** this batch.
- **`complete`** — **stateful**; holds the full result table and **rewrites the entire sink** every batch.

Spark enforces at query-start: `append` on a **non-watermarked** aggregation is **rejected** (it can't know when a key is "done"); `complete` and `update` **require** an aggregation.

**Why `complete` is dangerous at scale:** it rewrites the whole result every trigger — 10 M keys on a 5 s trigger means rewriting 10 M rows every 5 seconds. Production almost always uses **`update`** and lets a downstream `MERGE` / upsert handle it.
