## Checkpointing and fault tolerance

Checkpointing is what makes Structured Streaming **exactly-once** and **restartable** — without it, a crashed query restarts from scratch and may double-process. A checkpoint isn't just "last offset"; it's **three things**, advanced **atomically** every batch:

1. **Offsets** — source positions (file index, Kafka offset) — what's been *read*.
2. **WAL (write-ahead log)** — what's been *planned* for this batch — recovers in-flight data on a driver crash.
3. **State store snapshot** — running aggregates, window contents, join buffers.

All three move forward in **one atomic commit** per batch — there's no window where offsets advance but state doesn't. That atomicity *is* exactly-once.

**Always set `checkpointLocation`** on a production query. But end-to-end exactly-once needs **both** source and sink to support it: Kafka → Delta is exactly-once; Rate → memory is only at-least-once (the sink isn't durable); Socket → anything is at-most-once (the source isn't replayable).
