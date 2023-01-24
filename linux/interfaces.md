# Network interface

## Static network

For critical host you want static ip to keep connectivity even when the dhcp is down. Ex :

 - Promox host heberging the DHCP :)
 - Host used to debug


```
auto eth0
iface eth0 inet static
  address 10.x.x.242/24
  gateway 10.x.x.1
```

## DHCP

```
auto eth0
iface eth0 inet dhcp
```

## Bridge

```
auto vmbr0
iface vmbr0 inet static
        address  10.x.x.1/24
        bridge-ports none
        bridge-stp off
        bridge-fd 0

        post-up   echo 1 > /proc/sys/net/ipv4/ip_forward
        post-up   iptables -t nat -A POSTROUTING -s '10.x.x.0/24' -o vmbr0 -j MASQUERADE
        post-down iptables -t nat -D POSTROUTING -s '10.x.x.0/24' -o vmbr0 -j MASQUERADE
```