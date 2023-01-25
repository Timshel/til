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

## Systemctl list started services :

```bash
$systemctl list-dependencies multi-user.target
ulti-user.target
● ├─anacron.service
● ├─avahi-daemon.service
● ├─console-setup.service
● ├─cron.service
● ├─cups-browsed.service
● ├─cups.path
● ├─dbus.service
● ├─exim4.service
● ├─hddtemp.service
● ├─live-tools.service
● ├─lm-sensors.service
● ├─networking.service
● ├─plymouth-quit-wait.service
● ├─plymouth-quit.service
● ├─rsync.service
● ├─rsyslog.service
● ├─smartmontools.service
● ├─ssh.service
● ├─sysstat.service
● ├─systemd-ask-password-wall.path
● ├─systemd-logind.service
● ├─systemd-update-utmp-runlevel.service
● ├─systemd-user-sessions.service
● ├─basic.target
``` 