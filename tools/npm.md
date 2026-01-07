# Npm

## Disable scripts

Always disable scripts on install

```bash
>npm install --ignore-scripts
>npm ci --ignore-scripts
```

## Check for outdated dependencies

```bash
>npm outdated
Package      Current   Wanted  Latest  Location                  Depended by
@types/node  20.19.2  20.19.2  25.0.3  node_modules/@types/node  maildev
```
