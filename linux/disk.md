# Disk helpers


## To find a disk :


Source :

Last column (HCTL) identify the SATA port

```bash
$lsblk -o NAME,MODEL,SERIAL,HCTL
NAME                         MODEL                          SERIAL          HCTL
sda                          ST10000VN0004-1ZD101           ZA2.....        0:0:0:0
├─sda1
└─sda9
sdb                          ST10000VN0004-1ZD101           ZA2.....        1:0:0:0
├─sdb1
└─sdb9
sdc                          ST10000VN0004-1ZD101           ZA2.....        4:0:0:0
├─sdc1
└─sdc9
sdd                          ST10000VN0004-1ZD101           ZA2.....        5:0:0:0
├─sdd1
└─sdd9
sde                          ST10000VN0004-1ZD101           ZA2.....        10:0:0:0
├─sde1
└─sde9
sdf                          ST10000VN0004-1ZD101           ZA2.....        11:0:0:0
├─sdf1
└─sdf9
```



## Wipe a disk

### For HDD

```bash
shred -v -n 2  /dev/disk/by-id/ata-
```

### For SSD

- Check first if secure erase is supported: (If the `not` is not present),
- Need to set a password to be able to issue a secure erase command.
- Issue one of the two secure erase command.
- Check with `dd` that it output nothing.
- Disable the security

```bash
sudo hdparm -I /dev/disk/by-id/usb- | grep erase
sudo hdparm --user-master u --security-set-pass 'p' /dev/disk/by-id/usb-
sudo hdparm --user-master u --security-erase-enhanced 'p' /dev/disk/by-id/usb-
sudo hdparm --user-master u --security-erase 'p' /dev/disk/by-id/usb-
sudo hdparm --security-disable 'p' /dev/disk/by-id/usb-
sudo dd if=/dev/disk/by-id/usb- bs=1M count=5
```

## Hot remove a drive

Ensure that the disk is not used anymore (`unmount`, lvm ...)

Spin down the drive : ` hdparm -Y /dev/...`

Remove the device : `echo 1 | tee /sys/block/.../device/delete`
