# Covering indexes

A covering index is one that contains every column a query needs, so the query is answered from the index alone without ever touching the table. Normally the database uses the index to find matching rows, then does a second step (a lookup) to fetch the full row from the table. If the index already holds all the columns the query wants, that second step is skipped.

## An example

Take this query:

```sql
SELECT status FROM orders
WHERE customer_id = 42;
```

If you have an index on `(customer_id, status)`, the database can find `customer_id = 42` in the index and read `status` directly from the same index entry. It never reads the `orders` table itself. The index covers the query.

## Why it is faster

Skipping the table lookup avoids extra disk reads, which is often the slowest part of a query. For queries that return many rows, thousands of avoided lookups add up. Covering indexes are one of the biggest wins available when a specific query is hot and needs to be fast.

## Included columns

Some databases (SQL Server, Postgres, and others) let you add non-key columns to an index purely so it can cover queries, without those columns affecting the sort order:

```sql
CREATE INDEX idx_orders_cust
ON orders (customer_id) INCLUDE (status, total);
```

Here `customer_id` is the searchable key, while `status` and `total` ride along so `SELECT status, total WHERE customer_id = ?` is fully covered. The cost is the usual one: a wider index takes more space and more write effort, so cover the queries that matter, not every query.
