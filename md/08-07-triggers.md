## Triggers — controlling micro-batch cadence

A **trigger** defines how often Spark checks for new data and runs a micro-batch:

- **Default** (unspecified) — run the next batch as soon as the previous finishes.
- **Fixed interval** — `.trigger(processingTime="30 seconds")` — every 30 s; skip if the previous batch is still running.
- **Once** — `.trigger(once=True)` — process all available data in **one** batch, then stop.
- **Available now** — `.trigger(availableNow=True)` — process everything in **multiple** optimally-sized batches, then stop.
- **Continuous** — `.trigger(continuous="1 second")` — experimental ~1 ms-latency mode.

The key insight: **`once` and `availableNow` turn a streaming query into a self-terminating batch job** — you keep streaming semantics (exactly-once, offset tracking, idempotent restart) but the job exits once caught up. That's how one codebase serves both backfill and live processing.
