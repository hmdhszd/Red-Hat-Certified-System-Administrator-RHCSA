
`pv`    --->    physical volume      --->     disk/partition

`vg`    --->    volume group

`lv`    --->    logical volume


________________________________________________________________________________________________



install LVM


```bash
dnf install lvm2 -y
```

________________________________________________________________________________________________

## `lvmdiskscan`

to see overal `physical` `disk` status

```bash
[bob@centos-host ~]$ sudo lvmdiskscan

  /dev/vda1 [     <10.00 GiB] 
  /dev/vdb  [       1.00 GiB] 
  /dev/vdc  [       1.00 GiB] 
  /dev/vdd  [       1.00 GiB] 
  /dev/vde  [       1.00 GiB] 
  4 disks
  1 partition
  0 LVM physical volume whole disks
  0 LVM physical volumes
```

________________________________________________________________________________________________


## `pvcreate`


```bash
[bob@centos-host ~]$ sudo pvcreate /dev/vdc /dev/vdd /dev/vde /dev/vdb

  Physical volume "/dev/vdc" successfully created.
  Physical volume "/dev/vdd" successfully created.
  Physical volume "/dev/vde" successfully created.
  Physical volume "/dev/vdb" successfully created.
```

________________________________________________________________________________________________


## `pvscan`

```bash
[bob@centos-host ~]$ sudo pvscan

  PV /dev/vdb                      lvm2 [1.00 GiB]
  PV /dev/vdc                      lvm2 [1.00 GiB]
  PV /dev/vdd                      lvm2 [1.00 GiB]
  PV /dev/vde                      lvm2 [1.00 GiB]
  Total: 4 [4.00 GiB] / in use: 0 [0   ] / in no VG: 4 [4.00 GiB]
```

________________________________________________________________________________________________


## `pvs`

pvs — Display information about physical volumes

```bash
[bob@centos-host ~]$ sudo pvs

  PV         VG Fmt  Attr PSize PFree
  /dev/vdb      lvm2 ---  1.00g 1.00g
  /dev/vdc      lvm2 ---  1.00g 1.00g
  /dev/vdd      lvm2 ---  1.00g 1.00g
  /dev/vde      lvm2 ---  1.00g 1.00g
```

________________________________________________________________________________________________



## `pvdisplay`


```bash
[bob@centos-host ~]$ sudo pvdisplay

  --- Physical volume ---
  PV Name               /dev/vdb
  VG Name               volume1
  PV Size               1.00 GiB / not usable 4.00 MiB
  Allocatable           yes (but full)
  PE Size               4.00 MiB
  Total PE              255
  Free PE               0
  Allocated PE          255
  PV UUID               9uFIYZ-0vv6-soWU-Wwgz-Oeku-iKWZ-92Ta3W
   
  --- Physical volume ---
  PV Name               /dev/vdc
  VG Name               volume1
  PV Size               1.00 GiB / not usable 4.00 MiB
  Allocatable           yes 
  PE Size               4.00 MiB
  Total PE              255
  Free PE               126
  Allocated PE          129
  PV UUID               rBC0Wv-lEfW-td3c-UA6Q-f3br-L1K1-tf6dyG
   
  "/dev/vdd" is a new physical volume of "1.00 GiB"
  --- NEW Physical volume ---
  PV Name               /dev/vdd
  VG Name               
  PV Size               1.00 GiB
  Allocatable           NO
  PE Size               0   
  Total PE              0
  Free PE               0
  Allocated PE          0
  PV UUID               RqTBWv-c3mT-cb6d-fUyd-8Mdi-kSnY-bdcOlb
```

________________________________________________________________________________________________



## `pvremove`

pvremove — Remove LVM label(s) from physical volume(s)

```bash
[bob@centos-host ~]$ sudo pvremove /dev/vde

  Labels on physical volume "/dev/vde" successfully wiped.
```

________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ sudo pvscan

  PV /dev/vdb                      lvm2 [1.00 GiB]
  PV /dev/vdc                      lvm2 [1.00 GiB]
  PV /dev/vdd                      lvm2 [1.00 GiB]
  Total: 3 [3.00 GiB] / in use: 0 [0   ] / in no VG: 3 [3.00 GiB]
```

________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ sudo pvs

  PV         VG Fmt  Attr PSize PFree
  /dev/vdb      lvm2 ---  1.00g 1.00g
  /dev/vdc      lvm2 ---  1.00g 1.00g
  /dev/vdd      lvm2 ---  1.00g 1.00g
```

________________________________________________________________________________________________


## `vgcreate`

Create a Volume Group (VG)

```bash
[bob@centos-host ~]$ sudo vgcreate volume1 /dev/vdb /dev/vdc

  Volume group "volume1" successfully created
```

________________________________________________________________________________________________




## `vgdisplay`


```bash
[bob@centos-host ~]$ sudo vgdisplay
  --- Volume group ---
  VG Name               volume1
  System ID             
  Format                lvm2
  Metadata Areas        2
  Metadata Sequence No  4
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                1
  Open LV               0
  Max PV                0
  Cur PV                2
  Act PV                2
  VG Size               1.99 GiB
  PE Size               4.00 MiB
  Total PE              510
  Alloc PE / Size       384 / 1.50 GiB
  Free  PE / Size       126 / 504.00 MiB
  VG UUID               Gc9Vj2-CVHw-iul0-V7NU-JYbk-HHK6-UqVYQe
```

________________________________________________________________________________________________



## `vgscan`


```bash
[bob@centos-host ~]$ sudo vgscan

  Found volume group "volume1" using metadata type lvm2
```

________________________________________________________________________________________________


## `vgextend`

Add a physical volume to an existing Volume Group

```bash
[bob@centos-host ~]$ sudo vgextend volume1 /dev/vdd

  Volume group "volume1" successfully extended
```

________________________________________________________________________________________________


## `vgreduce`

Revome a PV from a VG

```bash
[bob@centos-host ~]$ sudo vgreduce volume1 /dev/vdd

  Removed "/dev/vdd" from volume group "volume1"
```

________________________________________________________________________________________________


## `vgs`

vgs — Display information about volume groups

```bash
[bob@centos-host ~]$ sudo vgs

  VG      #PV #LV #SN Attr   VSize VFree
  volume1   2   0   0 wz--n- 1.99g 1.99g
```

________________________________________________________________________________________________


## `lvcreate`

Create a Logical Volume (LV)

```bash
bob@centos-host ~]$ sudo lvcreate --name smalldata --size 1.5G volume1

  Logical volume "smalldata" created.
```

________________________________________________________________________________________________



## `lvs`


```bash
[bob@centos-host ~]$ sudo lvs

  LV        VG      Attr       LSize Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  smalldata volume1 -wi-a----- 1.50g 
```

________________________________________________________________________________________________


## `lvresize --resizefs --size`

Resize Logical Volume

```bash
[bob@centos-host ~]$ sudo lvresize --resizefs --size 1G /dev/volume1/smalldata

  WARNING: Reducing active logical volume to 1.00 GiB.
  THIS MAY DESTROY YOUR DATA (filesystem etc.)
Do you really want to reduce volume1/smalldata? [y/n]: yes
  Size of logical volume volume1/smalldata changed from 1.50 GiB (384 extents) to 1.00 GiB (256 extents).
  Logical volume volume1/smalldata successfully resized.
```

________________________________________________________________________________________________




## `lvs`

```bash
[bob@centos-host ~]$ sudo lvs

  LV        VG      Attr       LSize Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  smalldata volume1 -wi-a----- 1.00g  
```

________________________________________________________________________________________________


## `mkfs.xfs`

Create an XFS filesystem on the logical volume

```bash
[bob@centos-host ~]$ sudo mkfs.xfs /dev/volume1/smalldata

meta-data=/dev/volume1/smalldata isize=512    agcount=4, agsize=65536 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=1, sparse=1, rmapbt=0
         =                       reflink=1
data     =                       bsize=4096   blocks=262144, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0, ftype=1
log      =internal log           bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
```

________________________________________________________________________________________________


## `lvremove`

Destroy/Remove the Logical Volume

```bash
[bob@centos-host ~]$ sudo lvremove /dev/volume1/smalldata

Do you really want to remove active logical volume volume1/smalldata? [y/n]: yes
  Logical volume "smalldata" successfully removed.
```

________________________________________________________________________________________________
________________________________________________________________________________________________


## `lvresize --extents --size 100%VG`

resize LV, to 100%

```bash
[bob@centos-host ~]$ sudo lvresize --extents --size 100%VG /dev/volume1/smalldata
```

________________________________________________________________________________________________




## `lvscan`

```bash
[bob@centos-host ~]$ sudo lvscan

  ACTIVE            '/dev/volume1/smalldata' [1.50 GiB] inherit
```

________________________________________________________________________________________________




## `lvdisplay`

```bash
[bob@centos-host ~]$ sudo lvdisplay

  --- Logical volume ---
  LV Path                /dev/volume1/smalldata
  LV Name                smalldata
  VG Name                volume1
  LV UUID                a48M4D-FG6m-JX7u-f0L4-ecra-iBUu-ve1NF6
  LV Write Access        read/write
  LV Creation host, time centos-host, 2023-05-18 01:33:35 +0000
  LV Status              available
  # open                 0
  LV Size                1.50 GiB
  Current LE             384
  Segments               2
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     8192
  Block device           252:0
   
```

________________________________________________________________________________________________
