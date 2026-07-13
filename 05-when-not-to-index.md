# When not to index

Not every column deserves an index, and an unused or poorly chosen index is pure cost. Because each index slows writes and takes space, you should add one only when a real query benefits enough to justify that price. Several common situations argue against indexing.

## Small tables

On a table with a few hundred rows, a full scan is already fast because the whole table fits in memory and reading it is trivial. The database will often ignore an index on a small table and scan anyway, so the index just adds write overhead for no gain.

## Low-cardinality columns

Cardinality is how many distinct values a column has. A column like `is_active` with only two values (true and false), or `gender` with a handful, is low cardinality. An index there is rarely helpful: if half the rows match `is_active = true`, the database is often better off scanning than bouncing between the index and the table for half the table. Indexes pay off most on selective columns where a value matches a small fraction of rows.

## Heavily written, rarely queried columns

If a column is updated constantly but seldom filtered on, an index on it mostly slows the writes. Weigh how often the column is queried against how often it changes.

## Columns hidden by functions

An index on a plain column may be skipped if your query wraps the column in a function. `WHERE LOWER(email) = 'x'` cannot use a plain index on `email`, because the index stores the original values, not their lowercased form. Either index the expression (an expression or functional index) or store the data in the queried form. The lesson: index the columns your real queries filter on, in the form they filter, and drop indexes no query uses.
