## Default index types — the most-tested gotcha

Pandas DataFrames have a built-in row index; Spark DataFrames do **not** — rows have no inherent order. So Pandas-on-Spark must *fabricate* an index, and the strategy has performance and correctness implications. Three, set via `compute.default_index_type`:

- **`sequence`** — a single executor counts `0, 1, 2, ...`. Sequential (matches pandas) but **non-distributed** — slow on large data.
- **`distributed-sequence`** (the current default) — computed in parallel from partition offsets. Sequential, same numbers as pandas, slight overhead. **The right answer for most cases.**
- **`distributed`** — monotonically increasing ids. Cheapest and fully parallel, but **not sequential** — big gaps like `0, 1, 8589934592, ...`.

The trap is `distributed`: code that depends on exact index values — `loc` lookups, joins on the index, `.iloc` semantics — silently breaks. This is the single most-tested fact about the pandas API.
