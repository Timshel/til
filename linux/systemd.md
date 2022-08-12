# Systemd

## Add capabilities to service

Ref : https://blog.veloc1ty.de/2019/06/15/systemd-bind-to-privileged-port/

You can extend an existing service without changing it's source using a directory 
```bash
mkdir -p /etc/systemd/system/syncthing@.service.d 
```

Then adding `.conf` files ex `capabilities.conf` to allow to bind a priviledged port :
```
[Service]
AmbientCapabilities=CAP_NET_BIND_SERVICE
```

Don't forget to reload the service
```bash
systemctl daemon-reload
```