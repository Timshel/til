# Optimizations

## SSD access ([source](https://amplitude.engineering/how-a-single-postgresql-config-change-improved-slow-query-performance-by-50x-85593b8991b0))

When the query planner need to choose between a full scan and index access disk performance is taken into account.
`random_page_cost` and `seq_page_cost` control this. By default they are configured for HDD with respective value of (4 and 1).
When using and ssd the random `random_page_cost` cost is way lower, tuning this value become important for the query planner to make correct choice (A value of 1 might be indicated).

More [discussion](https://www.postgresql.org/message-id/51C36256.6040502%40optionshouse.com) and the official [doc](https://www.postgresql.org/docs/9.5/static/runtime-config-query.html).
