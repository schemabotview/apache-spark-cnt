## DataFrame API vs SQL — same plan

Write the same query both ways — a DataFrame chain and a `spark.sql(...)` string — and Catalyst rewrites **both into the identical physical plan**. The proof is `.explain()`: the two plans match line for line.

This is the payoff of the whole module. The choice between SQL and the DataFrame API is purely about **readability and team preference**, never performance:

- SQL shines for declarative, set-based logic and for people who already think in SQL.
- The DataFrame API shines for programmatic construction — building queries from variables, loops, and functions.

Mix them freely in one pipeline; the optimizer merges the plans regardless of which surface you used.
