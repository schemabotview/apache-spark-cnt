## Time, windows & watermarking

Two clocks: **processing time** (when Spark sees the row) and **event time** (when it actually happened — the honest clock, and what watermarking operates on). Three window types over event time:

- **Tumbling** — `window(ts, "5 minutes")` — fixed, non-overlapping.
- **Sliding** — `window(ts, "5 minutes", "1 minute")` — overlapping; each event lands in multiple windows.
- **Session** — `session_window(ts, "10 minutes")` — activity-driven; opens on the first event, closes after a gap of silence.

**Watermarking** handles late events: `watermark = max(event_time seen) − delay`. Events **before** the watermark are **dropped**; windows ending **at or before** it are **closed** — state freed, result emitted. Set it with `.withWatermark(col, "10 minutes")` **before** the `groupBy`. It's *required* for `append` on a windowed aggregation. The delay is a business SLA call — longer accepts more late data at the cost of memory and latency.
