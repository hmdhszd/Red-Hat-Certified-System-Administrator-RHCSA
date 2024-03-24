


## `Raid 0`:
(`sum` `2` disks)

1TB + 1TB = 2 TB


not redundant


________________________________________________________________________________________________




## `Raid 1`:
(`mirror` `2` disks)

1TB + 1TB = 1 TB



________________________________________________________________________________________________


## `Raid 5`:
(minimum `3` disks) (`parity`)
 
1TB + 1TB + 1TB = 2 TB


10 disks, 9 usable disks

if we `lose 1` disk of 10, we can still recover it




________________________________________________________________________________________________


## `Raid 6`:
 (minimum `4` disks) (`parity`)
 
 1TB + 1TB + 1TB + 1TB = 2 TB
 
if we `lose 2` disk of 4, we can still recover it




________________________________________________________________________________________________


## `Raid 10` (1+0): combination of 1 and 0

Raid 0 ( `2 disks (RAID1)` + `2 disks (RAID1)` )


not redundant


________________________________________________________________________________________________


install mdadm

```bash
[bob@centos-host ~]$ sudo yum install mdadm -y 
```

________________________________________________________________________________________________


to have raid, first we should get rid of vg and ps 

________________________________________________________________________________________________

## `mdadm --create /dev/md0 --level=1 --raid-devices=2`

create raid 1

```bash
[bob@centos-host ~]$ sudo mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/vdc /dev/vdd

mdadm: Note: this array has metadata at the start and
    may not be suitable as a boot device.  If you plan to
    store '/boot' on this device please ensure that
    your boot-loader understands md/v1.x metadata, or use
    --metadata=0.90
Continue creating array? yes
mdadm: Defaulting to version 1.2 metadata
mdadm: array /dev/md0 started.
```

Verify:

```bash
[bob@centos-host ~]$ lsblk

NAME   MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
vda    253:0    0   11G  0 disk  
└─vda1 253:1    0   10G  0 part  /
vdb    253:16   0    1G  0 disk  
vdc    253:32   0    1G  0 disk  
└─md0    9:0    0 1022M  0 raid1 
vdd    253:48   0    1G  0 disk  
└─md0    9:0    0 1022M  0 raid1 
vde    253:64   0    1G  0 disk 
```

________________________________________________________________________________________________


## `/proc/mdstat`

check raid on the system

```bash
[bob@centos-host ~]$ cat /proc/mdstat

Personalities : [raid6] [raid5] [raid4] [raid1] 
md0 : active raid1 vdd[1] vdc[0]
      1046528 blocks super 1.2 [2/2] [UU]
      
unused devices: <none>
```

________________________________________________________________________________________________





## `mdadm /dev/md0`

```bash
[bob@centos-host ~]$ sudo mdadm /dev/md0

/dev/md0: 1022.00MiB raid1 2 devices, 0 spares. Use mdadm --detail for more detail.
```


________________________________________________________________________________________________



## `mdadm --detail /dev/md0`



```bash
[bob@centos-host ~]$ sudo mdadm --detail /dev/md0
/dev/md0:
           Version : 1.2
     Creation Time : Sat Sep 23 19:49:26 2023
        Raid Level : raid1
        Array Size : 1046528 (1022.00 MiB 1071.64 MB)
     Used Dev Size : 1046528 (1022.00 MiB 1071.64 MB)
      Raid Devices : 2
     Total Devices : 2
       Persistence : Superblock is persistent

       Update Time : Sat Sep 23 19:49:32 2023
             State : clean 
    Active Devices : 2
   Working Devices : 2
    Failed Devices : 0
     Spare Devices : 0

Consistency Policy : resync

              Name : centos-host:0  (local to host centos-host)
              UUID : e8714ac6:213c25ad:de734b50:4b8ff0d7
            Events : 17

    Number   Major   Minor   RaidDevice State
       0     253       32        0      active sync   /dev/vdc
       1     253       48        1      active sync   /dev/vdd
```



________________________________________________________________________________________________

## `mdadm --manage /dev/md0 --add`

Add another device, /dev/vde, to the previously created array, /dev/md0 

```bash
[bob@centos-host ~]$ sudo mdadm --manage /dev/md0 --add /dev/vde

mdadm: added /dev/vde
```

________________________________________________________________________________________________



## `mkfs`


```bash
[bob@centos-host ~]$ sudo mkfs.xfs /dev/md0
```

________________________________________________________________________________________________


## `mdadm --stop`

`Stop` / `Deactivate` array

```bash
[bob@centos-host ~]$ sudo mdadm --stop /dev/md0
```

________________________________________________________________________________________________

## `mdadm --zero-superblock`

## erase the RAID metadata from a device

By using "--zero-superblock," you can erase the RAID metadata from a device, effectively making it available for other purposes or for creating a new RAID array.

This is useful when you want to repurpose a disk or troubleshoot RAID-related issues.

```bash
[bob@centos-host ~]$ sudo mdadm --zero-superblock /dev/sdb
```

________________________________________________________________________________________________

## `--spare-device`

add `spare disk` to an array

```bash
[bob@centos-host ~]$ sudo mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/vdc /dev/vdd --spare-device /dev/vde
```

________________________________________________________________________________________________


# Replace a raide device:


### first we should add the new device into the raid:

```bash
[bob@centos-host ~]$ sudo mdadm --manage /dev/md0 --add /dev/vde

mdadm: added /dev/vde
```

### them replace that with the old one

```bash
[bob@centos-host ~]$ sudo mdadm --manage /dev/md0 --replace /dev/vdd --with /dev/vde

mdadm: Marked /dev/vdd (device 1 in /dev/md0) for replacement
mdadm: Marked /dev/vde in /dev/md0 as replacement for device 1
```


```bash
[bob@centos-host ~]$ sudo mdadm --detail /dev/md0

/dev/md0:
           Version : 1.2
     Creation Time : Fri Dec 29 19:29:28 2023
        Raid Level : raid1
        Array Size : 1046528 (1022.00 MiB 1071.64 MB)
     Used Dev Size : 1046528 (1022.00 MiB 1071.64 MB)
      Raid Devices : 2
     Total Devices : 3
       Persistence : Superblock is persistent

       Update Time : Fri Dec 29 19:34:12 2023
             State : clean 
    Active Devices : 2
   Working Devices : 2
    Failed Devices : 1
     Spare Devices : 0

Consistency Policy : resync

              Name : centos-host:0  (local to host centos-host)
              UUID : 7906a082:44c26f04:a1e6b459:878cf886
            Events : 22

    Number   Major   Minor   RaidDevice State
       0     253       32        0      active sync   /dev/vdc
       2     253       64        1      active sync   /dev/vde

       1     253       48        -      faulty   /dev/vdd
```

