# DDRescue

https://www.linux.com/topic/desktop/gnu-ddrescue-best-damaged-drive-rescue/

First we try to copy files when it works.
We then retry multiple times the ones which failed.

```bash
$ sudo ddrescue -f /dev/sdb1 /dev/sdc1 logfile
$ sudo ddrescue -f -r3 /dev/sdb1 /dev/sdc1 logfile
```