## API parity — what actually changes

Once the mental model clicks, the code surface is small. Batch and streaming use the **same DataFrame transformations** — `filter`, `select`, `groupBy`, `join`, `agg`, windows. Only the input/output boundary differs. Exactly **five** things change:

1. **`spark.readStream`** instead of `spark.read` — plus an **explicit schema** (Spark can't infer from an unbounded source).
2. **`df.writeStream`** instead of `df.write`.
3. **`outputMode(...)`** — which rows to emit per micro-batch.
4. **`option("checkpointLocation", ...)`** — where to persist offsets + state + WAL.
5. **`start()`** returns a `StreamingQuery` handle (`write` returned `None`).

Everything *between* the read and the write is identical DataFrame code. That's the parity — and why the same logic powers both historical backfill and live processing with minimal change.
