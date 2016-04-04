# Access a remote machine visible only through another

Netcat need to be installed on the intermetdiate host (bastion)

```
Host prod
        Hostname prod
        Port 22
        Protocol 2
        User root
        ProxyCommand ssh root@bastion "nc -w 10 %h %p"
```