# Disk Burnin

Cf https://blog.linuxserver.io/2018/10/29/new-hard-drive-rituals/

## Badblocks

Check all device block. It will perform 4 complete write and read cycles across the entire drive, the last of which zeroes the drive.
Run `smartctl` before and after.

```bash
$ badblocks -b 4096 -wsv /dev/sdX
```
