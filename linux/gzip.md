## Reproductible archives

By default `gzip` include timestamp in created file. To prevent it :

```bash
GZIP=-n tar -caf archive.tgz folder/
```