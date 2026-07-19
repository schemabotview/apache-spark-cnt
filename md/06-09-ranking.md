## Ranking — `row_number`, `rank`, `dense_rank`, `ntile`

Four ranking functions over a window — the difference is entirely in **how they handle ties**. For three rows where the first two tie:

- **`row_number()`** — unique sequential; ties broken arbitrarily → `1, 2, 3`.
- **`rank()`** — tied rows share the rank, then the next rank **skips** → `1, 1, 3`.
- **`dense_rank()`** — tied rows share the rank, **no gap** → `1, 1, 2`.
- **`ntile(n)`** — assigns rows to one of `n` quantile buckets.

The classic use is "top-N per group": `row_number()` over `partitionBy(group).orderBy(desc(metric))`, then filter to `rank <= N`.
