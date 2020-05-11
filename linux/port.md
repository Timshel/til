# Port checking

Netstat : list open port for tcp and udp :
``` bash
netstat -ltup
```

Check if a UDP port is open (not always reliable, check with a port you know is closed):
```bash
nc -vz -u x.x.x.x 9988
``` 