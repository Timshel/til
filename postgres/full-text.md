# Basic full text search in postgres

Cf:
 - https://www.postgresql.org/docs/9.5/static/textsearch.html
 - http://rachbelaid.com/postgres-full-text-search-is-good-enough/

Add a column with will store the `tsVector`, and add an index
```
ALTER TABLE details ADD COLUMN search tsvector;
update details set search = to_tsvector('english', id || ' ' || coalesce(name, ''))
CREATE INDEX io_details_search ON io.details USING GIN (search)
to_tsvector
```

Select a value ordered with the rank
```
select ts_rank_cd(search, query) as rank, *
  from details, to_tsquery('english', '"LG & HOMBOT"') query
  where search @@  query
  order by rank desc
  limit 10
```

If you don't care about accent add package `postgresql-contrib-9.x` to use `unaccent` and then :

```
CREATE EXTENSION IF NOT EXISTS unaccent;
update details set search = to_tsvector('english', id || ' ' || unaccent(name))
```
