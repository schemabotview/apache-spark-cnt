## What a DataFrame really is

A DataFrame is a **distributed table**: one schema — column names and types — shared across the whole dataset, plus rows partitioned across executor memory. A useful shorthand: **a DataFrame is an RDD with metadata**, where the column names are human-friendly aliases for fields and Catalyst is the translation layer between the two.

The partitioning is always **horizontal** — each partition holds a *subset of rows with all their columns*, never a subset of columns. That's what lets Spark process partitions in parallel across executors.

Four properties define it:

- **Immutable** — every transformation returns a new DataFrame.
- **Lazy** — transformations build a plan; an action executes it.
- **Schema-enforced** — every row matches the same column types.
- **Partitioned** — rows split across partitions for parallel processing.
