# What an index is

A database index is a separate sorted structure that maps column values to the rows that hold them, so the database can find matching rows without reading the whole table. Without an index, answering `WHERE email = 'x'` means a full table scan: the database reads every row and checks each one. With an index on `email`, it jumps straight to the matching entries.

## The book analogy

Think of a printed book. To find every mention of a word, you could read all 400 pages (a full scan), or you could flip to the index at the back, find the word, and go directly to the listed page numbers. The book index is sorted alphabetically and points into the content. A database index is the same idea: a sorted copy of one or more columns, with pointers back to the full rows.

## The B-tree

Most indexes are B-tree indexes (technically B+ trees). A B-tree keeps values sorted and balanced across layers of nodes, so a lookup walks from the root down to a leaf in a small number of steps. For a table with a million rows, a B-tree finds a value in roughly twenty comparisons instead of a million. Because the leaves are sorted and linked, B-trees are also fast for range queries like `WHERE age BETWEEN 20 AND 30` and for `ORDER BY` on the indexed column.

## What it speeds up

An index helps queries that filter (`WHERE`), join (matching a key in another table), or sort (`ORDER BY`) on the indexed columns. The database consults the small sorted index, collects the matching row locations, then fetches those rows. The payoff grows with table size: on a tiny table a scan is fine, but on a large one the index is the difference between milliseconds and seconds.
