## Gotchas — where the pandas API ends

The surface is *close* to pandas, but not identical — and the exam loves the differences:

- **Row order** — not guaranteed unless you `sort_index()` / `sort_values()` (pandas is stable).
- **Lazy, not eager** — only actions trigger execution.
- **`inplace=True`** — often ignored; Spark DataFrames are immutable.
- **`iterrows` / `df.apply(axis=1)` without a type hint** — pull to the driver or sample to infer schema — slow and brittle.
- **`df.loc[missing]`** — may silently return empty instead of raising `KeyError`.
- **Some methods** raise `PandasNotImplementedError`.

Two settings to know: **`compute.ops_on_diff_frames`** — `False` by default, so combining two pandas-on-Spark frames raises an error (pandas' row-alignment needs a shared index Spark can't guarantee without a shuffle); set `True` only when you've reasoned about the cost. And **`compute.default_index_type`** — from the last section.
