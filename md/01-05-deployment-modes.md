## Deployment modes & cluster managers

The cluster manager decides how the driver and executors actually get placed onto machines. You choose it with a master URL when you build the session.

- **`local[*]`** — everything in one JVM on your laptop; the `*` means "use all available cores." This is the mode used for learning, and for every notebook in this course.
- **Standalone** — Spark's own built-in cluster manager. Simple, with no external dependency.
- **YARN** — runs on Hadoop clusters; common in legacy on-prem stacks.
- **Kubernetes** — driver and executors run as pods; the modern cloud-native default.
- **Databricks** — managed Spark, where the cluster manager is hidden behind the platform.

For learning, `local[*]` is plenty. For the exam, the thing to recognize is each name and what it runs on — you rarely configure them by hand.
