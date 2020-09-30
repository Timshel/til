# Kubernetes

## Force stop pod :

First try to force it :

```bash
> kubectl delete pod kube-flannel-ds-amd64-2hzcj  --grace-period=0 --force --namespace kube-system
```

If it does not work :

```bash
> kubectl patch pod kube-flannel-ds-amd64-2hzcj  -p '{"metadata":{"finalizers":null}}' -n kube-system
```

## Dns resolution

Cf:

- https://www.weave.works/blog/racy-conntrack-and-dns-lookup-timeouts
- https://www.digitalocean.com/community/tutorials/how-to-inspect-kubernetes-networking

Issues with dns resolution which fail half the time.

To check the iptables rules with the ip then identify the correct service.
```bash
$ sudo iptables-save | grep 10.96.0.10
$ sudo iptables-save | grep -E "KUBE-SVC-TCOU7JCQXEZGVUNU"
$ sudo iptables-save | grep -E "KUBE-SEP-SG5KFJDSTWXMSQHM|KUBE-SEP-MQQPF3ICZZDLLJF3"
```

We check the contract and see that the entry is correctly created bu we have no reponse.
```bash
$ sudo conntrack -E -d 10.96.0.10
```

We use tcpdump to check that traffic is sent through the `flannel` interface, and the physical one.
```bash
$ sudo tcpdump -ni flannel.1 host 10.244.9.0
$ sudo tcpdump -ni enp4s0 host 192.168.1.247 | grep -A 2 -B 1 overlay
```