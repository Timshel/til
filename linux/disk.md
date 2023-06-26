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

```bash
shred -v -n 2  /dev/disk/by-id/ata-
```


## Hot remove a drive

Ensure that the disk is not used anymore (`unmount`, lvm ...)

Spin down the drive : ` hdparm -Y /dev/...`

Remove the device : `echo 1 | tee /sys/block/.../device/delete`
