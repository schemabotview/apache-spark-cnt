## Join strategies, broadcast, and when NOT to broadcast

Spark picks a join algorithm from the table sizes and any hints:

- **Broadcast Hash Join** — one side ≤ `autoBroadcastJoinThreshold` (default 10 MB) or a `broadcast()` hint. **No shuffle** — the small side ships to every executor and the join runs locally. The single biggest win for **star-schema** queries.
- **Sort-Merge Join** — both sides large; shuffles both. The default for big fact-to-fact joins.
- **Shuffle Hash Join** — one side fits in memory per partition; shuffles one side.
- **Cartesian / Nested Loop** — cross joins only.

**Force a broadcast** by wrapping the small side in `broadcast(df)` (or the `/*+ BROADCAST(t) */` SQL hint). But **know when not to**: auto below 10 MB, safe to force up to ~100 MB, test between 100–500 MB, and **never broadcast > 500 MB** — a broadcast that doesn't fit executor memory OOMs the cluster.
