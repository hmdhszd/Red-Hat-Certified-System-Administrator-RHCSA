

## `fdisk`

```bash
sudo fdisk /dev/vdb
```


________________________________________________________________________________________________




## `mkfs`


```bash
sudo mkfs.vfat /dev/vdb1
```

________________________________________________________________________________________________





## `mount`

```bash
sudo mount /dev/vdb1 /mnt
```

________________________________________________________________________________________________




## `lsblk`


```bash
[bob@centos-host ~]$ lsblk

NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
vda    253:0    0   11G  0 disk 
└─vda1 253:1    0   10G  0 part /
vdb    253:16   0    1G  0 disk 
└─vdb1 253:17   0 1023M  0 part /mnt
vdc    253:32   0    1G  0 disk 
vdd    253:48   0    1G  0 disk 
vde    253:64   0    1G  0 disk 
```

________________________________________________________________________________________________



## `/etc/fstab`

make it permanent

```bash
[root@centos-host bob]# echo "/dev/vdb1 /mnt vfat defaults 0 0" >> /etc/fstab 

[root@centos-host bob]# mount -a

[root@centos-host bob]# systemctl daemon-reload
```

default       -->     mount option

next 0        -->     `Backup` --> (`0`=disable/`1`=enable)

next 0        -->     `Scan` file system at boot --> (`0`=disable/`2`=enable)




________________________________________________________________________________________________



## `blkid`

we can use UUID instead of /dev/vdb1

UUID="a62c5b49-755e-41b0-9d36-de3d95e17232" /mnt vfat defaults 0 0

```bash
[bob@centos-host ~]$ sudo blkid /dev/vda1
/dev/vda1: UUID="a62c5b49-755e-41b0-9d36-de3d95e17232" BLOCK_SIZE="512" TYPE="xfs" PARTUUID="ef431952-01"
```

________________________________________________________________________________________________

### add a swap in the fstab


```bash
[root@centos-host bob]# echo "/dev/vdb3 none swap defaults 0 0" >> /etc/fstab 

[root@centos-host bob]# mount -a

[root@centos-host bob]# systemctl daemon-reload
```

and verify the swap:




```bash
swapon --show
```

________________________________________________________________________________________________
