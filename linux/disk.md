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

#### HDParm secure erase

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

#### DD

```bash
udo dd if=/dev/zero of=/dev/disk/by-id/usb-  bs=4096 status=progress
```

#### Blkdiscard

```bash
blkdiscard -sz /dev/zero of=/dev/disk/by-id/usb-
```

## Hot remove a drive

Ensure that the disk is not used anymore (`unmount`, lvm ...)

Spin down the drive : ` hdparm -Y /dev/...`

Remove the device : `echo 1 | tee /sys/block/.../device/delete`


## Disk sleep

### Checking state

To check the state without waking up the drives (work for standby but not for sleep)

```bash
smartctl --nocheck standby -i /dev/sda
```

### Manually

```bash
hdparm -Y /dev/sdx
```

### Configuration

Configure 1h sleep time :
```bash
sudo hdparm -S 241 /dev/sda
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

### Seagate

For disks with no APM settings (ex ironwolf) better to use [openSeaChest_PowerControl](https://github.com/Seagate/openSeaChest).

#### Check settings

```bash
>./openSeaChest_PowerControl -d /dev/sda --showEPCSettings
Name       Current Timer Default Timer Saved Timer   Recovery Time C S
Idle A     *1            *1            *1            1             Y Y
Idle B     *1200         *1200         *1200         4             Y Y
Idle C      0             6000          6000         50            Y Y
Standby Z   0             9000          9000         120           Y Y
```

#### Change the settings

```bash
>./openSeaChest_PowerControl -d /dev/sda --idle_c 600000
>./openSeaChest_PowerControl -d /dev/sda --standby_z 900000
```

## Fun with GPT tables

### Check current table

``` bash
>gdisk -l  /dev/mapper/pve-vm--200--disk--0
```

If corrupted will probably prompt to try to use the table and output some potential informations.

### Fixing it ?

Gpart is able to scan the disk and may propose some fix.

```bash
gpart /dev/mapper/pve-vm--200--disk--0
```

In my case it started to ouput valid information but was slow.

### Copy and replace it

If you have access to a similar table (easy when the disk come from an image) then the easiest solution is probably to use sfdisk to export the valid one and restore it on the broken disk.

```bash
> sfdisk -d /dev/zvol/stock/pve/vm-200-disk-4 > ha.gpt
> sfdisk /dev/mapper/pve-vm--200--disk--0  < ha.gpt
```
