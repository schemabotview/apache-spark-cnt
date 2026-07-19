## Reading plans with `.explain()`

`df.explain(mode="...")` exposes the plans Catalyst built — the single best way to see *why* a query is slow. Five modes, each a different lens on the same query:

- **`simple`** (default) — just the physical plan.
- **`extended`** — all four plans, parsed to analyzed to optimized to physical.
- **`codegen`** — the generated JVM code per stage.
- **`cost`** — the optimized plan with row-count estimates per node.
- **`formatted`** — the physical plan plus a separate operator-details section.

```python
df.groupBy("category").count().explain(mode="formatted")
```

Read the physical plan **bottom-up**: the scan is at the bottom, and data flows upward through each operator. Two things to look for — an `Exchange` node marks a shuffle (a stage boundary), and a `*(N) WholeStageCodegen` wrapper marks operators Tungsten fused into one loop.
