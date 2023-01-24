# ZFS

## Pool status :

```bash
$zpool status
NAME                                   STATE     READ WRITE CKSUM
stock                                  ONLINE       0     0     0
  raidz2-0                             ONLINE       0     0     0
    ata-ST10000VN0004-1ZD101_ZA.....   ONLINE       0     0     1  block size: 512B configured, 4096B native
    ata-ST10000VN0004-1ZD101_ZA.....   ONLINE       0     0     0  block size: 512B configured, 4096B native
    ata-ST10000VN0004-1ZD101_ZA.....   ONLINE       0     0     0  block size: 512B configured, 4096B native
    ata-ST10000VN0004-1ZD101_ZA.....   ONLINE       0     0     0  block size: 512B configured, 4096B native
    ata-ST10000VN0004-1ZD101_ZA.....   ONLINE       0     0     0  block size: 512B configured, 4096B native
    ata-ST10000VN0004-1ZD101_ZA.....   ONLINE       0     0     0  block size: 512B configured, 4096B native

```

## Find disk ID :

```bash
$ls -1 /dev/disk/by-id/ | grep ata
```

## Wipe disk :

Without `-a` command return the headers informations.

```bash
$wipefs -a  /dev/disk/by-id/ata-TOSHIBA_HDWG21C_...
```

## To recreate gpt partition table :

```bash
$parted  /dev/disk/by-id/ata-TOSHIBA_HDWG21C_...
>mklabel GPT
>Yes
```

## Replace an existing drive with the spare (need to specify `ashift` since native for drive is `4096B`) :

```bash
sudo zpool replace -o ashift=9 stock ata-ST10000VN0004-1ZD101_... ata-TOSHIBA_HDWG21C_...
```

## Detach the old drive :

```bash
sudo zpool remove stock ata-ST10000VN0004-1ZD101_...
```