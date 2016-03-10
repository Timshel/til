# Stop network manager from overriding resolv.conf

On debian the resolv.conf file is updated by network-manager.

An easy solution is to make the file immutable, as root :
```
chattr +i /etc/resolv.conf
```

Other possible solutions : http://www.cyberciti.biz/faq/dhclient-etcresolvconf-hooks/