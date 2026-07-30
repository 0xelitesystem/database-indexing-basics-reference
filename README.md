# Database Indexing Basics

A plain-language reference on database indexes: what they are, how they make reads fast, what they cost on writes and storage, and how to decide which columns to index. It covers B-tree indexes, single versus composite indexes, covering indexes, when not to index, and how to confirm an index is actually used.

## The core idea

A database index is a separate, sorted data structure that lets the database find rows without scanning the whole table. It works like the index at the back of a book: instead of reading every page, you jump straight to the entry. Indexes make reads that filter or sort on the indexed columns dramatically faster, but every index adds cost to inserts, updates, and deletes, and takes disk space. The skill is indexing the columns your queries actually filter and sort on, in the right order, and nothing more.

## What is inside

- [01-what-an-index-is.md](01-what-an-index-is.md) the book-index analogy and what a B-tree does.
- [02-the-cost-of-indexes.md](02-the-cost-of-indexes.md) the write and storage price you pay for faster reads.
- [03-composite-indexes-and-column-order.md](03-composite-indexes-and-column-order.md) multi-column indexes and why order matters.
- [04-covering-indexes.md](04-covering-indexes.md) serving a whole query from the index alone.
- [05-when-not-to-index.md](05-when-not-to-index.md) cases where an index does not help or hurts.
- [06-checking-with-explain.md](06-checking-with-explain.md) using EXPLAIN to see if an index is used.

## The stance

An index is a tradeoff, not free speed. This reference treats indexing as a deliberate choice driven by the queries you run, measured with EXPLAIN, not guessed at by adding an index to every column.

## More

Part of a catalog of single-file browser tools and plain-language references, all MIT licensed and dependency-free: [0xelitesystem.github.io](https://0xelitesystem.github.io/). Built by [elitesystem.ai](https://elitesystem.ai).

## License

MIT. Copyright 0xelitesystem 2026.
