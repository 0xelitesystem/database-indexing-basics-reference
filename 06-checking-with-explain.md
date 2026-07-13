# Checking with EXPLAIN

`EXPLAIN` shows the plan the database will use to run a query, including whether it uses an index or scans the whole table. Instead of guessing whether your index helps, you ask the database directly. Put `EXPLAIN` in front of a query and read the plan it prints.

## Reading the plan

```sql
EXPLAIN SELECT * FROM orders WHERE customer_id = 42;
```

The output describes how the query will run. The key thing to look for is the access method. Terms vary by database, but the pattern is the same:

- A sequential scan or full table scan means every row is read. On a large table filtered by a selective condition, this is the sign that an index is missing or not being used.
- An index scan or index seek means the database is using an index to find rows. This is usually what you want for a selective filter.

In MySQL the plan has a `type` column (`ALL` means full scan, `ref` or `range` means index use) and a `key` column naming the index chosen. In Postgres you read `Seq Scan` versus `Index Scan` in the plan text.

## EXPLAIN ANALYZE

`EXPLAIN` alone estimates the plan; `EXPLAIN ANALYZE` actually runs the query and reports real timings and row counts. Use it to see where time is truly spent and to catch cases where the database estimated one thing but reality differed.

## Why the index might be ignored

If you added an index and the plan still shows a full scan, common causes are: the table is too small for the index to matter, the column is low cardinality so the scan is cheaper, the filter wraps the column in a function, statistics are stale (run `ANALYZE`), or the query matches a large fraction of the table. EXPLAIN turns indexing from guesswork into measurement: change something, run EXPLAIN, and confirm the plan improved.
