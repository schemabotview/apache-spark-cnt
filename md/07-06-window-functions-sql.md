## Window functions in SQL

The Window API from module 06, in SQL syntax. The whole spec collapses into one `OVER (...)` clause:

```sql
SELECT *, ROW_NUMBER() OVER (
    PARTITION BY customer_id
    ORDER BY txn_date
    ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
) AS rn
FROM transactions
```

Same semantics as the DataFrame version — `ROW_NUMBER()`, `RANK()`, `DENSE_RANK()`, `NTILE(n)`, `LAG(col, n)`, `LEAD(col, n)`, and running aggregates over a frame. The three parts map one-to-one: `PARTITION BY` = `partitionBy`, `ORDER BY` = `orderBy`, and `ROWS`/`RANGE BETWEEN` = the frame. If you know the DataFrame window, you already know the SQL window.
