# Wireguard :)

Resources :

https://www.jordanwhited.com/posts/wireguard-endpoint-discovery-nat-traversal/


## Install

```bash
apt-get install wireguard wireguard-tools net-tools linux-headers-`uname -r`
cd /etc/wireguard/
wg genkey | tee privatekey | wg pubkey > publickey
```


## Server Conf :

```conf
[Interface]
Address = 10.0.0.1/24
SaveConfig = false
ListenPort = 42820
PrivateKey = $ServPrivateKey
PostUp = iptables -I INPUT 9 -p udp -i enp3s0 --dport 42820 -j ACCEPT
PostUp = iptables -I INPUT 10 -i %i -j ACCEPT
PostUp = iptables -I FORWARD 1 -i %i -o enp3s0 -j ACCEPT
PostUp = iptables -I FORWARD 2 -i enp3s0 -o %i -m state --state ESTABLISHED,RELATED -j ACCEPT
PostUp = iptables -I FORWARD 3 -i %i -o vmbr0 -j ACCEPT
PostUp = iptables -I FORWARD 4 -i vmbr0 -o %i -m state --state ESTABLISHED,RELATED -j ACCEPT
PostUp = iptables -I FORWARD 5 -i %i -o %i -j ACCEPT
PostUp = iptables -t nat -A POSTROUTING -o enp3s0  -j MASQUERADE --source 10.0.0.0/24
PostUp = ip route add 10.42.0.0/16 dev %i via 10.0.0.42 
PostUp = ip route add 10.128.0.0/16 dev %i via 10.0.0.128
PostDown = iptables -D INPUT -p udp -i enp3s0 --dport 42820 -j ACCEPT
PostDown = iptables -D INPUT -i %i -j ACCEPT
PostDown = iptables -D FORWARD -i %i -o enp3s0 -j ACCEPT
PostDown = iptables -D FORWARD -i enp3s0 -o %i -m state --state ESTABLISHED,RELATED -j ACCEPT
PostDown = iptables -D FORWARD -i %i -o vmbr0 -j ACCEPT
PostDown = iptables -D FORWARD -i vmbr0 -o %i -m state --state ESTABLISHED,RELATED -j ACCEPT
PostDown = iptables -D FORWARD -i %i -o %i -j ACCEPT
PostDown = iptables -t nat -D POSTROUTING -o enp3s0  -j MASQUERADE --source 10.0.0.0/24
PostDown = ip route del 10.42.0.0/16
PostDown = ip route del 10.128.0.0/16

[Peer]
#Phone
PublicKey = $PeerPublicKey
AllowedIPs = 10.0.0.2/32

```


## Client conf :

Generate key : 
```bash
umask 077; wg genkey | tee privatekey | wg pubkey > publickey
``` 


```conf
[Interface]
Address = 10.0.0.48/24
SaveConfig = false
ListenPort = 42820
PrivateKey = $PeerPrivateKey
DNS = 10.1.1.11

[Peer]
#Serveur
PublicKey = $ServPublicKey
Endpoint = 5.135.178.35:42820
AllowedIPs = 0.0.0.0/0
PersistentKeepalive = 25
```