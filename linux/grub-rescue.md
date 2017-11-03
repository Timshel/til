# Repair boot failure from grub rescue

First boot

```
ls
set prefix=(hd0,5)/boot/grub
set root=(hd0,5)
set
ls /boot
insmod /boot/grub/x86_64-efi
linux /vmlinuz root=/dev/sda5 ro
initrd /initrd.img
boot
```

Then 
```
sudo update-grub
sudo grub-install /dev/sda
```

If the grub-install fail with `error: efibootmgr failed to register the boot entry: Input/output error.`
```
grub-install -v --target=x86_64-efi --recheck /dev/sda
```
