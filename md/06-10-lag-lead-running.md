## `lag` / `lead` & running aggregates

Two families of window functions built on the frame:

- **Offset — `lag` / `lead`** — within a partition, `lag(col, n, default)` looks **back** `n` rows and `lead(col, n, default)` looks **forward** `n` rows, returning the default (or `null`) at the boundary. Perfect for deltas: "days since previous transaction," "time to next event."
- **Running aggregates — the frame** — `rowsBetween` counts **physical row positions**; `rangeBetween` counts the **value range** of the order-by column (e.g. "all rows within ±7 days"). Three frames worth memorizing:
  - **Cumulative**: `rowsBetween(unboundedPreceding, currentRow)`
  - **N-row trailing**: `rowsBetween(-N, currentRow)`
  - **Centered**: `rowsBetween(-N, N)`

`rows` vs `range` is the most-tested window distinction — physical offset vs value offset.
