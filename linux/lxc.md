# LXC setup / helper

## Create the container

Source : https://wiki.debian.org/fr/LXC

```bashs
sudo lxc-create -n whatsapp -t debian -- -r buster
```

## Network using libvirt

Source : https://wiki.debian.org/fr/LXC/LibVirtDefaultNetwork

```bash
apt-get install libvirt-daemon-system libvirt-clients
reboot
virsh net-info default
```

Add network interfaces :

```bash
$ sudo virsh net-edit default 
<network>
  <name>default</name>
  ...
  <ip address='192.168.122.1' netmask='255.255.255.0'>
    <dhcp>
      <range start='  192.168.122.2' end='192.168.122.254'/>
      <host mac='00:FF:AA:00:00:02' ip='192.168.122.2'/>
      ...
    </dhcp>
  </ip>
</network>
```

Start virsh interface

```bash
virsh net-start default
ip a
virsh net-autostart default
```

## Lxc config

Update the `lxc` config with a specified mac (`/var/lib/lxc/whatsapp/config`)
n
```
...
# Container specific configuration
lxc.tty.max = 4
lxc.pty.max = 1024
lxc.uts.name = whatsapp
lxc.arch = amd64
lxc.start.auto = 1

# Network conf
lxc.net.1.type = veth
lxc.net.1.flags = up
lxc.net.1.link = virbr0
lxc.net.1.hwaddr = 00:FF:AA:00:00:06
lxc.net.1.ipv4.address = 192.168.122.6/24
lxc.net.1.ipv4.gateway = 192.168.122.1
```

Set `eth0` to static ip

```bash
$sudo nano /var/lib/lxc/whatsapp/etc/network/interfaces
iface eth0 inet static
```

## IPTables config

Ex forward 2222 to container port 22 and accept established connections (`PREROUTING` is applied before `FORWARD` so ensure to use correct `dport`)

```bash
# whatsapp routing
/sbin/iptables -A FORWARD -i virbr0 -s 192.168.122.6 -j ACCEPT

/sbin/iptables -t nat -A PREROUTING -i eno1 -p tcp --dport 2222 -j DNAT --to 192.168.122.6:22
/sbin/iptables -A FORWARD -i eno1 -p tcp -d 192.168.122.6 --dport 22 -j ACCEPT

# masquerade
iptables -t nat -A POSTROUTING -o eno1 -j MASQUERADE
```

## Run the container

```bash
$sudo lxc-start -d -n whatsapp
$sudo lxc-attach -n whatsapp
```

## X11 ?

Cf: https://blog.simos.info/running-x11-software-in-lxd-containers/
Wayland: https://discuss.linuxcontainers.org/t/howto-use-the-hosts-wayland-and-xwayland-servers-inside-containers/8765
