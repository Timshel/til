# Pagination with total number

Use window function to get the total results count
```
SELECT
    COUNT(*) OVER () AS total_entries_count,
    id, name
  FROM details
    WHERE search = '800555'
  LIMIT 10;
```
