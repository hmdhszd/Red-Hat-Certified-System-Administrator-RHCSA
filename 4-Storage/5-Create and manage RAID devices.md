


Raid 0: (add 2 disks)

1TB + 1TB = 2 TB


not redundant


________________________________________________________________________________________________




Raid 1: (mirror 2 disks)

1TB + 1TB = 1 TB



________________________________________________________________________________________________


Raid 5:
 (minimum 3 disks) (parity)
 
1TB + 1TB + 1TB = 2 TB


10 disks, 9 usable disks

if we loose 1 disk of 10, we can still recover it




________________________________________________________________________________________________


Raid 6:
 (minimum 4 disks) (parity)
 
 
 
if we loose 2 disk of 4, we can still recover it




________________________________________________________________________________________________


Raid 10 (1+0): combination of 1 and 0

Raid 0 ( 2 disks (RAID1) + 2 disks (RAID1) )


not redundant


________________________________________________________________________________________________


to have raid, first we should get rid of vg and ps 

create raid 0

```bash
mdadm --create 
```

________________________________________________________________________________________________




```bash
mkfs
```

________________________________________________________________________________________________


Stop / Deactivate array

```bash
adadm --stop 
```

________________________________________________________________________________________________


By using "--zero-superblock," you can erase the RAID metadata from a device, effectively making it available for other purposes or for creating a new RAID array.
This is useful when you want to repurpose a disk or troubleshoot RAID-related issues.

```bash
mdadm --zero-superblock /dev/sdb
```

________________________________________________________________________________________________


add spare disk to an array

```bash
--spare-device
```

________________________________________________________________________________________________




```bash
manage --add
```

________________________________________________________________________________________________


check raid on the system

```bash
cat /proc/mdstat
```

________________________________________________________________________________________________
