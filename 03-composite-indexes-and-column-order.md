# Composite indexes and column order

A composite index covers two or more columns in one index, and the order of those columns decides which queries it can help. A composite index is sorted by the first column, then by the second within each value of the first, and so on, exactly like sorting a list of people by last name and then first name.

## An example

Suppose you often run:

```sql
SELECT * FROM orders
WHERE customer_id = 42 AND status = 'shipped';
```

You create:

```sql
CREATE INDEX idx_orders_cust_status
ON orders (customer_id, status);
```

This index is sorted first by `customer_id`, then by `status`. To answer the query, the database jumps to `customer_id = 42`, and within that block the rows are already sorted by `status`, so `status = 'shipped'` is found immediately.

## The leftmost-prefix rule

A composite index can be used for a query only if the query filters on a leftmost prefix of its columns. The index on `(customer_id, status)` helps queries that filter on `customer_id` alone, or on `customer_id` and `status` together. It does not help a query that filters only on `status`, because the index is not sorted by `status` first. This is the leftmost-prefix rule, and it is the single most important thing to know about composite indexes.

## Choosing the order

Put the column used in the most queries, and used for equality (`=`) rather than ranges, first. A range condition like `WHERE created_at > '2026-01-01'` on an early column stops the index from being useful for later columns, so equality columns generally belong before range columns. Order for your query patterns, not alphabetically.
