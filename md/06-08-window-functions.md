## Window functions — concept and spec

A **window function** computes a value *per row* using a frame of neighboring rows — and unlike `groupBy`, **the original rows survive** (no collapse). That's the whole point: rank, running total, or "value vs. previous row" *alongside* the raw data.

A window spec has three parts:

1. **`partitionBy(...)`** — groups rows; the frame resets at each partition boundary.
2. **`orderBy(...)`** — defines row order within a partition.
3. **Frame** — `rowsBetween` (physical row offsets) or `rangeBetween` (value offsets along the order-by column).

Defaults differ by function: ranking functions ignore the frame; aggregate functions default to the running frame. The boundary constants are `Window.unboundedPreceding`, `Window.currentRow`, and `Window.unboundedFollowing`.
