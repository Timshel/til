# OsX routing

view current routes :

```bash
$netstat -nr
Routing tables

Internet:
Destination        Gateway            Flags        Refs      Use   Netif Expire
default            192.168.0.1        UGSc           82        3     en5
127                127.0.0.1          UCS             0        0     lo0
127.0.0.1          127.0.0.1          UH             13    54828     lo0
169.254            link#7             UCS             2        0     en5
...
```

Add some route:
```bash
$route add -net 192.168.1.1/32 x.x.x.x
$route add -net 192.168.1 -interface utun1
```

Delete some route:
```bash
$route delete -net 192.168.1
$route delete -net 192.168.1.12/32
```