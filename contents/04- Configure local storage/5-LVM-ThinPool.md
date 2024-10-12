

# ThinPool and ThinVolume
________________________________________________________________________________________________

#### Create a 5T thin provisioned volume "mythinvol" under the 700M thin pool "mythinpool" in the 800M volume group "myvg".


```bash
[root@centos-host bob]$ lsblk

NAME   MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
vda    253:0    0  11G  0 disk 
└─vda1 253:1    0  10G  0 part /
vdb    253:16   0   1G  0 disk 
vdc    253:32   0   1G  0 disk 
vdd    253:48   0   1G  0 disk 
vde    253:64   0   1G  0 disk
```


________________________________________________________________________________________________

## 1.  Format the disk:

```bash
[root@centos-host bob]$ fdisk /dev/vdb

Welcome to fdisk (util-linux 2.32.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table.
Created a new DOS disklabel with disk identifier 0xe74aa9e3.

Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): 

Using default response p.
Partition number (1-4, default 1): 
First sector (2048-2097151, default 2048): 
Last sector, +sectors or +size{K,M,G,T,P} (2048-2097151, default 2097151): +800M

Created a new partition 1 of type 'Linux' and of size 800 MiB.






Command (m for help): t
Selected partition 1
Hex code (type L to list all codes): 8e
Changed type of partition 'Linux' to 'Linux LVM'.





Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.
```


Verify:

```bash
[root@centos-host bob]$ lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
vda    253:0    0   11G  0 disk 
└─vda1 253:1    0   10G  0 part /
vdb    253:16   0    1G  0 disk 
└─vdb1 253:17   0  800M  0 part 
vdc    253:32   0    1G  0 disk 
vdd    253:48   0    1G  0 disk 
vde    253:64   0    1G  0 disk
```



________________________________________________________________________________________________


## 2.  Create a PV:

```bash
[root@centos-host bob]$ pvcreate /dev/vdb1
```


________________________________________________________________________________________________

## 3.  Create a VG:

```bash
[root@centos-host bob]$ vgcreate myvg /dev/vdb1
```


________________________________________________________________________________________________

## 4.  Create a thin pool: `--thinpool`

```bash
[root@centos-host bob]$  lvcreate --name mythinpool --type thin-pool --size 700M myvg
```




________________________________________________________________________________________________

## 5.  Create thinly-provisioned logical volume: `--thin` `--virtualsize`

```bash
[root@centos-host bob]$ lvcreate --name mythinvol --thin /dev/myvg/mythinpool --virtualsize 5T
```

Verify:

```bash
[root@centos-host bob]$ lvs
  LV         VG   Attr       LSize   Pool       Origin Data%  Meta%  Move Log Cpy%Sync Convert
  mythinpool myvg twi-aotz-- 700.00m                   0.00   10.94                           
  mythinvol  myvg Vwi-a-tz--   5.00t mythinpool        0.00                             
```



  
________________________________________________________________________________________________





## Extend the size of "mythinpool" by 50M.


```bash
[root@centos-host bob]$ lvextend /dev/myvg/mythinpool --size +50M
```

```bash
[root@centos-host bob]$ lvs
  LV         VG   Attr       LSize   Pool       Origin Data%  Meta%  Move Log Cpy%Sync Convert
  mythinpool myvg twi-aotz-- 752.00m                   0.00   10.94                           
  mythinvol  myvg Vwi-a-tz--   5.00t mythinpool        0.00     
```



________________________________________________________________________________________________



## Rename the thin pool from "mythinpool" to "thinpool1".


```bash
[root@centos-host bob]$ lvrename /dev/myvg/mythinpool /dev/myvg/mythinpool1
  Renamed "mythinpool" to "mythinpool1" in volume group "myvg"
```

```bash
[root@centos-host bob]$ lvs
  LV          VG   Attr       LSize   Pool        Origin Data%  Meta%  Move Log Cpy%Sync Convert
  mythinpool1 myvg twi-aotz-- 752.00m                    0.00   10.94                           
  mythinvol   myvg Vwi-a-tz--   5.00t mythinpool1        0.00                                   
```



________________________________________________________________________________________________


## Rename the thin provisioned volume from "mythinvol" to "thinvol1".


```bash
[root@centos-host bob]$ lvrename /dev/myvg/mythinvol /dev/myvg/mythinvol1
```

```bash
[root@centos-host bob]$ lvs
  LV          VG   Attr       LSize   Pool        Origin Data%  Meta%  Move Log Cpy%Sync Convert
  mythinpool1 myvg twi-aotz-- 752.00m                    0.00   10.94                           
  mythinvol1  myvg Vwi-a-tz--   5.00t mythinpool1        0.00            
```





________________________________________________________________________________________________



