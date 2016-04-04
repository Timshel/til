# Create a tunnel from a local port to a remote port

Example to access a database

```
ssh -N -L 6432:host:5432 root@bastion
```

`-N` : Dot not execute remote command
`-L` : [bind_address:]port:host:hostport