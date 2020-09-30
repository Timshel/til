# OsX DNS config

To view current dns config (`/etc/resolv.conf` is legacy and only used by command line) :

```bash
$scutil --dns
>
```

To add a custom resolver for a domain ex  `/etc/resolver/toto.fr`

```
nameserver 10.10.1.1
nameserver 10.10.1.2
```

Those resolvers will be used only for `toto.fr` and subdomains.

## Dnsmasq

If you need custom resolver even when using command line, you can run a local dns which will forward the queries.
Simple brew install :

```bash
brew install dnsmasq
````

A simple config to forward different domains :

```
server=/toto.fr/10.10.1.1
server=/toto.plop.com/1.1.1.1
server=/plop.com/135.228.27.80
server=/plop.com/135.228.27.84
server=/plop.com/1.1.1.1
server=//1.1.1.1
```

Restart service:

```bash
 sudo launchctl stop homebrew.mxcl.dnsmasq; sudo launchctl start homebrew.mxcl.dnsmasq
 ```

 And update `/etc/resolv.conf` to point locally. Sadly even trying to protect against write the file `sudo chflags noschg  /etc/resolv.conf` do not prevent the system to overwrite it.