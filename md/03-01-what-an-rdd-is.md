## What an RDD actually is

An **RDD** — Resilient Distributed Dataset — is the original Spark data abstraction. Every DataFrame you write today eventually compiles down to RDD operations under the hood, so understanding RDDs explains *why* Spark behaves the way it does: lazy evaluation, fault tolerance through lineage, and partition-level parallelism.

Read the three letters of the name:

- **Resilient** — fault-tolerant. Lost partitions are recomputed from the recipe (lineage), not retrieved from a replica.
- **Distributed** — partitioned across executor memory on many machines.
- **Dataset** — a collection of records. A record can be any object — a string, a tuple, a custom class. Unlike a DataFrame, there is **no schema and no column names**.

Four more properties worth memorizing: an RDD is **immutable** (every transformation makes a new RDD), **lazy** (transformations are recorded, nothing runs until an action), **schema-free** (just elements — you handle structure in code), and **typed** at runtime (fully type-parameterized in Scala, untyped at the language level in Python).
