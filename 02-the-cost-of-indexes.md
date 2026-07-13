# The cost of indexes

An index speeds up reads but slows down writes and consumes storage, so it is a tradeoff, not free performance. Every index is a second data structure the database must keep in sync. When you insert, update, or delete a row, the database updates the table and every affected index, so the more indexes a table has, the slower its writes.

## The write cost

On an `INSERT`, each index on the table must add the new value in its correct sorted position. On an `UPDATE` that changes an indexed column, the old index entry is removed and a new one inserted. On a `DELETE`, entries are removed from every index. A table with eight indexes pays that maintenance eight times on each write. For write-heavy tables (logging, event streams, queues), extra indexes can noticeably hurt throughput.

## The storage cost

An index is a real copy of the indexed columns plus pointers, so it takes disk space and memory. A composite index on three wide columns can approach the size of the table data itself. More index data also means less of the useful data fits in the database cache, which can push reads back to disk.

## The balance

The rule of thumb: index the columns your queries filter, join, and sort on, and stop there. An index that no query uses is pure cost, slowing writes and wasting space while helping nothing. Periodically review indexes and drop the ones no query touches. The goal is the smallest set of indexes that covers your real query patterns.
