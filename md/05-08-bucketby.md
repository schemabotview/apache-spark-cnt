## `bucketBy` — bucketing for join-friendly layouts

`df.write.bucketBy(n, "col")` hashes rows into `n` files per partition, fixing the column's value-to-file mapping. Two payoffs:

- A later **join** on that column can skip the shuffle if both sides are bucketed identically — same `n`, same key.
- **Aggregations** on the bucket column can run per-bucket, with no network step.

The catch: `bucketBy` only works with **`saveAsTable`** — it needs the metastore to record the bucketing metadata. A plain `save(path)` ignores it entirely.
