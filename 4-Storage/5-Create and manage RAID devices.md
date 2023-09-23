


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
[bob@centos-host ~]$ sudo mkfs /dev/md0
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

## `/proc/mdstat`

check raid on the system

```bash
cat /proc/mdstat
```

________________________________________________________________________________________________
