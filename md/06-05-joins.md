## Joins — types, syntax, and the duplicate-column trap

`df1.join(df2, on, how)`. Six `how` values: **`inner`** (default), **`left`** / **`right`** / **`full`** (outer, null-fill the missing side), **`left_semi`** (rows of `df1` *with* a match — no `df2` columns), and **`left_anti`** (rows of `df1` *without* a match). Plus `cross` for the Cartesian product.

Two ways to name the key — and the trap lives here:

- **String** — `df1.join(df2, "k")` when both sides share the column name. Spark **de-duplicates** the join column.
- **Expression** — `df1.join(df2, df1.x == df2.y, ...)` when names differ — both columns survive.

**The trap:** the expression form on same-named columns (`df1.k == df2.k`) keeps **two** columns called `k`, so a later `select("k")` raises *"Reference 'k' is ambiguous."* Use the string form when names match. And note joins use null-unsafe equality — null keys never match; use `eqNullSafe` if you need them to.
