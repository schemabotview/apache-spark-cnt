## Tuning `spark.sql.shuffle.partitions`

After any wide transformation, the number of output partitions defaults to **200** — a value sized for large clusters. On `local[*]` with 8 cores that's ~192 mostly-empty partitions and a pile of scheduling overhead; on a big dataset it can be far too *few*, giving oversized partitions that spill.

The fix depends on the setting:

- **Local mode** — set it to **2–4× your core count**.
- **A real cluster** — target the **128–256 MB-per-partition** rule against your *largest expected post-shuffle dataset*.

`spark.conf.set("spark.sql.shuffle.partitions", n)` does it. Note that **AQE now coalesces** empty post-shuffle partitions automatically (next section), so this matters most when the default is too *small*, not too large.
