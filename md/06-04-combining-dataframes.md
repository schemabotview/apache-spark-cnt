## Combining DataFrames — `union` vs `unionByName`

Both stack two DataFrames row-wise; the difference is how they *pair columns*:

- **`df1.union(df2)`** — pairs columns **positionally**, by index. Names are ignored. If the schemas don't line up positionally but the types are compatible, it **silently mixes the wrong columns** — no error.
- **`df1.unionByName(df2)`** — pairs columns **by name**. A missing column raises `AnalysisException`, unless you pass `allowMissingColumns=True` to fill the gap with `null`.

**Prefer `unionByName`** unless you specifically need positional pairing — the exam loves this trap, and so does production data. (The set operations `intersect` and `except` also stack vertically, comparing whole rows.)
