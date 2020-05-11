# Disk conf

Configure 1h sleep time :
```
sudo hdparm -S 242 /dev/sda
```

```
The encoding of the timeout value is somewhat peculiar.  
A value of zero means "timeouts are disabled": the device will not automatically enter standby mode.  
Values from 1 to  240  specify multiples of 5 seconds, yielding timeouts from 5 seconds to 20 minutes.  
Values from 241 to 251 specify from 1 to 11 units of 30 minutes, yielding timeouts from 30 minutes to5.5 hours.
```

To persist it, add in `/etc/hdparm.conf`
```
command_line {
  hdparm -S 241 /dev/disk/by-id/XXXX
}
```