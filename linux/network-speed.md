# Trooblshoot network speed :

## Check connection

```bash
>ethtool eth0
Settings for eth0:
	Supported ports: [ TP ]
	Supported link modes:   10baseT/Half 10baseT/Full
	                        100baseT/Half 100baseT/Full
	                        1000baseT/Full
	Supported pause frame use: Symmetric
	Supports auto-negotiation: Yes
	Supported FEC modes: Not reported
	Advertised link modes:  10baseT/Half 10baseT/Full
	                        100baseT/Half 100baseT/Full
	                        1000baseT/Full
	Advertised pause frame use: Symmetric
	Advertised auto-negotiation: Yes
	Advertised FEC modes: Not reported
	Speed: 1000Mb/s
	Duplex: Full
	Port: Twisted Pair
	PHYAD: 1
	Transceiver: internal
	Auto-negotiation: on
	MDI-X: off (auto)
```

Speed seem ok.

## Use iperf to check speed between two server :

Create a server on one of the two
```bash
> iperf -s
```

Run the client on the other :
```bash
> iperf -c host1 --dualtest --num --time 30
[  6] local 192.168.1.149 port 51724 connected with 192.168.1.188 port 5001
[  4]  0.0-30.0 sec  2.56 GBytes   732 Mbits/sec
[  6]  0.0-30.1 sec  3.27 GBytes   935 Mbits/sec
```

### with ipv6 :

Server side :
```bash
> iperf -s -V
```
Client :
```bash
> iperf -c fe80::...%interface -V --dualtest --num --time 30
[  6] local 192.168.1.149 port 51724 connected with 192.168.1.188 port 5001
[  4]  0.0-30.0 sec  2.56 GBytes   732 Mbits/sec
[  6]  0.0-30.1 sec  3.27 GBytes   935 Mbits/sec
```