## Default indexes — the most-tested gotcha

Pandas DataFrames have a built-in row index. Spark DataFrames do **not** — rows have no inherent order. So Pandas-on-Spark has to *fabricate* an index when you don't give it one, and the strategy it picks has real performance and correctness implications. Three strategies, set via `compute.default_index_type`:

- **`sequence`** — a single executor counts `0, 1, 2, ...`. Sequential (matches pandas), but triggers a **non-distributed** step — slow on large data.
- **`distributed-sequence`** (the current default) — computed in parallel from partition offsets. Sequential, same numbers as pandas, slight overhead. The right answer for most cases.
- **`distributed`** — monotonically increasing ids. **Cheapest** and fully parallel, but **not sequential** — you get gaps like `0, 1, 8589934592, ...`.

The trap: pandas code that depends on exact index values — loc-based lookups, joins on the index, `.iloc` semantics — silently breaks under `distributed`. Set it explicitly:

```python
ps.set_option("compute.default_index_type", "distributed-sequence")
```
