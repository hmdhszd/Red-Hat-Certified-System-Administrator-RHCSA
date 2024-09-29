
## `fdisk`

### create

press
t     type
b     FAT32

```bash
sudo fdisk /dev/sdb
```

OR



## `mkfs.vfat`

```bash
mkfs.vfat /dev/sdb
```

by `default` it's `16 bit` and you can have up to 2G file system


________________________________________________________________________________________________


## `mkfs.vfat -F 32`

if you want to have bigger file system use `32 bit`

```bash
mkfs.vfat -F 32 /dev/sdb
```

________________________________________________________________________________________________


## `mount`

```bash
mkdir /myvfat
mount  /dev/sdb /myvfat
```

________________________________________________________________________________________________


## `nano /etc/fstab`

add an entry in the /etc/fstab

```bash
/dev/sdb /myvfat vfat default 0 0
```

________________________________________________________________________________________________


### un-mount


```bash
umount /myvfat
```

________________________________________________________________________________________________
