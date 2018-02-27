# Kill a client connection 

Kill all connections to the db `demo` :
```
SELECT pg_terminate_backend(pid) from pg_stat_activity WHERE datname = 'demo';
```
