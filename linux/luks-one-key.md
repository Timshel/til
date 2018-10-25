# One passphrase multiple partitions

From : https://www.martineve.com/2012/11/02/luks-encrypting-multiple-partitions-on-debianubuntu-with-a-single-passphrase/

## Create the partition
```
sudo gdisk /dev/sda
> o # create GPT table
> w #write changes

sudo fdisk /dev/sda
> n # create a new partition
> 1 # number of the partition
> 4096 # first block
> w # write the change
```

## Create the luks Volume
When creating the file system `-m 0` disable reserved block for root.
```
sudo cryptsetup luksFormat -c aes-xts-plain64 -s 512 -h sha512 /dev/sda1
sudo cryptsetup luksOpen /dev/sda1 data_crypt
sudo mkfs.ext4  -m 0  /dev/mapper/data_crypt
```

## Create a keyfile
The keyfile allow to unlock `data_crypt` without the passphrase.
```
mkdir /mnt/keys
sudo dd if=/dev/urandom of=/mnt/keys/data.keyfile bs=1024 count=4
sudo chmod 0400 data.keyfile
sudo cryptsetup luksAddKey /dev/sda1 /mnt/keys/data.keyfile
```
## Configure automount
list mountpoint with UUID : 
```
> lsblk -o name,uuid,mountpoint
sda
└─sda1                  56334250-a327-4472-8fbf-5144550621c6
  └─data_crypt          fa44ed67-2e57-414a-83bc-8a8cf990bdbc   /mnt/data
```

Dans `/etc/crypttab`
```
data_crypt UUID=56334250-a327-4472-8fbf-5144550621c6  /mnt/keys/data.keyfile luks
```

Dans `/etc/fstab`
```
# Data drive
/dev/mapper/data_crypt /mnt/data           ext4    defaults        0       2
```
