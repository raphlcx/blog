---
title: "Book: SQL Performance Explained"
date: 2024-11-04T21:11:19+08:00
---

Notes from the book *SQL Performance Explained*, by Markus Winand, 2012.

- B-tree index performance bottleneck usually comes from scanning too many leaf nodes, and having to perform table access
  - For multi-column index, place the more selective column as the first index column
  - For wildcard search, ensure the wildcard does not appear at the beginning of filter values
  - Use partial index to avoid indexing values that are never queried
  - Avoid indexing columns with NULL values
  - Use range when querying date
  - Use the right data type for each column: avoid storing number as string
  - Avoid unnecessary filters
  - Avoid performing maths on filters
- SQL join statement only joins two tables at a single time
- Indexing join predicates doesn’t improve hash join performance, but indexing independent predicates does
- Hash join is more efficient by optimising memory usage, rather than number of rows
- Select fewer columns to improve hash join performance
- Clustering data means to store consecutively accessed data closely together so that accessing it requires fewer IO operations
- Don’t introduce a new index for the sole purpose of filter predicates. Extend an existing index instead.
- An index-only scan allows fetching all necessary data without having to access the tables
- Avoid select * and fetch only the columns you need
- If the index order corresponds to the order by clause, the database can omit the explicit sort operation
- Group will perform sort if grouping is done using sort/group algorithm, hence indexing on sort columns can help
- The database can only optimize a query for a partial result if it knows this from the beginning e.g. query using LIMIT
- Pagination using seek/cursor method can work better, since it can utilise index access without having to scan through the past pages as with the offset method
- Paging requires a deterministic sort order
- Adding index negatively affects insert, update, and delete performance, although these operations do benefit slightly from indices on the predicates i.e. WHERE clauses
