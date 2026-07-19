## Transformations — lazy, return another `ps.DataFrame`

Every operation here returns a *new* pandas-on-Spark frame **without triggering a Spark job** — exactly like Spark DataFrame transformations, they build up a plan. An action (next section) is what fires the job. The toolkit divides into six families:

- **Column ops** — selection, assignment, derived columns.
- **Filtering** — boolean masks and `query()`.
- **Aggregation** — `groupby` + `agg`.
- **Joins** — `merge`.
- **Cleanup** — `fillna`, `dropna`, `astype`, `rename`, `sort_values`.
- **Per-element work** — the `.str` and `.dt` accessors, plus `apply` / `transform` (pass a **type hint** to skip the schema-inference sampling).

The mental model from module 02 holds unchanged: nothing runs until an action.
