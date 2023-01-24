# Port checking

Netstat : list open port for tcp and udp (package `net-tools`) :
``` bash
netstat -ltupn
```

## Net cat


Check if a UDP port is open (not always reliable, check with a port you know is closed):
```bash
nc -vz -u x.x.x.x 9988
```

Open a TCP listener on the server side :

```bash
nc -l 8000
```

You can then send some message using :

```bash
$nc 127.0.0.1 8000
> Hello
```

You can switch to UDP using `-u` in both command.
