# Chroot

## To rescue a system

Mount the partition

```bash
mkdir -p /mnt/rescue
mount /dev/sda2 /mnt/rescue
mount --bind /dev /mnt/rescue/dev
mount -t proc /proc  /mnt/rescue/proc
mount -t sysfs /sys /mnt/rescue/sys
chroot /mnt/rescue /bin/bash
```