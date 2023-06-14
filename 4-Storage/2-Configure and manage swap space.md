

to see info of the swap partition

```bash
[bob@centos-host ~]$ swapon --show

NAME      TYPE SIZE USED PRIO
/swapfile file   2G 8.8M   -2
```

________________________________________________________________________________________________


to add swap, first we make a new partition

```bash
[bob@centos-host ~]$ sudo cfdisk /dev/vdb
```

________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
vda    253:0    0   11G  0 disk 
└─vda1 253:1    0   10G  0 part /
vdb    253:16   0    1G  0 disk 
└─vdb1 253:17   0 1023M  0 part 
```

________________________________________________________________________________________________


then change the partition to become a swap

```bash
[bob@centos-host ~]$ sudo mkswap /dev/vdb1
Setting up swapspace version 1, size = 1023 MiB (1072668672 bytes)
no label, UUID=c58dd8d2-f61d-48a7-88de-2c6d7076c836
```

________________________________________________________________________________________________


finally, add that partition to the machine

```bash
[bob@centos-host ~]$ sudo swapon /dev/vdb1

[bob@centos-host ~]$ sudo swapon --show
 
NAME      TYPE       SIZE USED PRIO
/swapfile file         2G 6.8M   -2
/dev/vdb1 partition 1023M   0B   -3
```

this swap will be used until restart of the machine


________________________________________________________________________________________________


swapon, swapoff - enable/disable devices and files for paging and swapping

```bash
[bob@centos-host ~]$ sudo swapoff /dev/vdb1
```


```bash
[bob@centos-host ~]$ sudo swapon --show
 
NAME      TYPE       SIZE USED PRIO
/swapfile file         2G 6.8M   -2
```

________________________________________________________________________________________________


# Create a swap file to use permanently

use file instead for swap of partition


```bash
bob@centos-host ~]$ sudo dd if=/dev/zero of=/swap bs=1M count=128 status=progress

128+0 records in
128+0 records out
134217728 bytes (134 MB, 128 MiB) copied, 0.0729469 s, 1.8 GB/s
```


```bash
[bob@centos-host ~]$ ls -ltrh /swap
-rw-r--r--. 1 root root 128M May 15 01:04 /swap
```

________________________________________________________________________________________________


limit the swap for other users

```bash
[bob@centos-host ~]$ sudo chmod 600 /swap
```

________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ sudo mkswap /swap

Setting up swapspace version 1, size = 128 MiB (134213632 bytes)
no label, UUID=4bf780ab-4324-4afd-adfe-892748f4787e
```


```bash
[bob@centos-host ~]$ sudo swapon /swap
```


```bash
[bob@centos-host ~]$ sudo swapon --show

NAME      TYPE SIZE USED PRIO
/swapfile file   2G 9.8M   -2
/swap     file 128M   0B   -3
```

________________________________________________________________________________________________
