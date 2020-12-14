# Some Docker tips

## Docker ignore whitelist :

Start the `.dockerignore` file by excluding everyting :

```
*
!poetry.lock
!pyproject.toml
```

## Smaller images

### Apt

To ensure smaller layer in the same `RUN` update and at the end remove the source list cache.

```bash
RUN apt-get update && apt-get install -y --no-install-recommends \
    cuda-cudart-10-2=10.2.89-1 \
    cuda-compat-10-2 \
    rm -rf /var/lib/apt/lists/*
```