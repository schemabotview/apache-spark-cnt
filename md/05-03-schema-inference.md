## Schema inference vs explicit

Two ways to get a schema when reading CSV or JSON — and they are not equal:

- **Explicit schema** — fast (no scan), type-safe (guaranteed), nulls declared per column. For production pipelines.
- **Inferred schema** — slow (a full or sampled scan *before* your query runs), a best-effort type guess, everything assumed nullable. For exploratory work only.

Inference triggers a *separate scan over the data* before the real job starts — on a 100 GB CSV that's a non-trivial pre-job. Worse, a single malformed row can silently change the inferred type: `credit_limit` was a `decimal` yesterday, but today one row holds the string `"NA"`, so it's now a `string`, and every downstream `sum()` fails.

**Always pass an explicit schema in production.**
