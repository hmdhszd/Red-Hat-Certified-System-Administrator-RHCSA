



```bash
[bob@centos-host ~]$  lsblk
NAME   MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
vda    253:0    0  11G  0 disk 
└─vda1 253:1    0  10G  0 part /
vdb    253:16   0   1G  0 disk 
vdc    253:32   0   1G  0 disk 
vdd    253:48   0   1G  0 disk 
vde    253:64   0   1G  0 disk 
```

________________________________________________________________________________________________


list partitions

```bash
[bob@centos-host ~]$ sudo fdisk --list /dev/vda

Disk /dev/vda: 11 GiB, 11811160064 bytes, 23068672 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xef431952

Device     Boot Start      End  Sectors Size Id Type
/dev/vda1  *     2048 20971519 20969472  10G 83 Linux
```

________________________________________________________________________________________________


for managing disks with GUI

```bash
[bob@centos-host ~]$ sudo cfdisk /dev/vdb
```


________________________________________________________________________________________________
