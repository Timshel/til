# Resize Partition and FS

Disk partition info :
```
>lsblk
sda      8:0    0 447.1G  0 disk 
├─sda4   8:4    0     2M  0 part 
├─sda2   8:2    0 446.6G  0 part 
├─sda3   8:3    0   512M  0 part 
└─sda1   8:1    0     1M  0 part 
```

Resize FS first when shrinking :
```
>resize2fs /dev/sda2 100G
```

Run partition using `parted` :
```
>parted

>select /dev/sda

>print
Partition Table: gpt
Disk Flags: 

Number  Start   End     Size    File system     Name       Flags
 1      1049kB  2097kB  1049kB                  bios_grub  bios_grub
 2      2097kB  480GB   480GB   ext4            primary
 3      480GB   480GB   537MB   linux-swap(v1)  primary
 4      480GB   480GB   2080kB                  logical

>resizepart
Partition number?
>2
End ?
> 108GB
```

Resize FS to match underlaying blocks (GB / G mismatch) :
```
>resize2fs /dev/sda2
```

If `resize2fs` complain of mismatch block size (you shrinked too much :() fix your partition size using `parted`.

If `/etc/fstab` contain uuid when creating new partition use `blkid`.