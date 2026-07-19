## Adaptive Query Execution (AQE)

Before Spark 3.0, Catalyst made every decision *upfront* from *estimated* statistics — and estimates are often wrong. **AQE re-optimizes the plan at runtime**, after each shuffle, using the *real* partition statistics it just measured.

Three things AQE actually does:

- **Dynamically coalesce partitions** — if a shuffle produces 200 partitions but most are tiny, AQE merges them into a smaller number of right-sized partitions, cutting task-startup overhead.
- **Switch join strategy at runtime** — a sort-merge join can be promoted to a broadcast join if one side turns out small enough once measured.
- **Handle skew** — it splits oversized partitions so one slow task doesn't stall a whole stage. (Detail lands in the performance-tuning module.)

The key mental model: this is the *same Catalyst optimizer*, but given a second chance **at each stage boundary** with facts instead of guesses. AQE is on by default since Spark 3.2.
