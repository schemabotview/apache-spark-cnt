## Detecting and fixing data skew

**Data skew** is when a few keys dominate the partition distribution. Because a stage can't finish until its *last* task does, one oversized partition stalls the whole stage. Symptoms:

- One task runs **10–100× longer** than the median (Spark UI → Stages).
- One executor shows GC pressure or disk spill while the others sit idle.
- The stage finishes fast except for the last one or two tasks.

Three fixes, in order of preference:

1. **AQE skew join** — the modern default (Spark 3.2+, `skewJoin.enabled = true`). It detects skewed partitions at runtime and splits them. **Try this first.**
2. **Salting** — append a random suffix to the hot key on the large side and explode the small side N times to match. The most reliable fix when AQE isn't enough.
3. **`repartitionByRange(n, col)`** — split into equal-sized ranges, balanced regardless of key frequency.

The same disease appears in `groupBy` — same medicine: salt the key and aggregate twice (partial, then final).
