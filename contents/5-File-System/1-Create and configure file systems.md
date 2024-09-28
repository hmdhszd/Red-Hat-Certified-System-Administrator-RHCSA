

## `mkfs`

```bash
[bob@centos-host ~]$ mkfs
mkfs         mkfs.ext2    mkfs.ext4    mkfs.xfs     
mkfs.cramfs  mkfs.ext3    mkfs.minix  
```

________________________________________________________________________________________________

## `mkfs.xfs`

`-L`   -->   `set Label`

`-i size=`   -->   `inode size`


```bash
[bob@centos-host ~]$ sudo mkfs.xfs -L "MyNewVolume" -i size=512 /dev/vdb
```


________________________________________________________________________________________________


## `xfs_`

xfs utilities

```bash
[bob@centos-host ~]$ xfs_
xfs_admin      xfs_estimate   xfs_info       xfs_metadump   xfs_repair
xfs_bmap       xfs_freeze     xfs_io         xfs_mkfile     xfs_rtcp
xfs_copy       xfs_fsr        xfs_logprint   xfs_ncheck     xfs_spaceman
xfs_db         xfs_growfs     xfs_mdrestore  xfs_quota  
```

________________________________________________________________________________________________


## `xfs_admin -l`

xfs_admin for modify filesystem

`-l` --> `show current label`


```bash
[bob@centos-host ~]$ sudo xfs_admin -l /dev/vdb

label = "MyNewVolume"
```

________________________________________________________________________________________________



## `xfs_admin -L`

`-L`   -->   `set Label`

```bash
[bob@centos-host ~]$ sudo xfs_admin -L "MyNewLabel" /dev/vdb

writing all SBs
new label = "MyNewLabel"
```

________________________________________________________________________________________________


## `mkfs.ext4 -N`

### mke2fs - create an ext2/ext3/ext4 filesystem    =   mkfs.ext4

-N      number of `inodes`

```bash
[bob@centos-host ~]$ sudo mkfs.ext4 -N 500000 /dev/vdd

mke2fs 1.45.6 (20-Mar-2020)
Creating filesystem with 262144 4k blocks and 500224 inodes
Filesystem UUID: 0bbbee5d-06fc-4972-b54e-f578ac0e269b
Superblock backups stored on blocks: 
        17336, 52008, 86680, 121352, 156024

Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (8192 blocks): done
Writing superblocks and filesystem accounting information: done 
```

________________________________________________________________________________________________


## `tune2fs -l`

### tune2fs - adjust tunable filesystem parameters on ext2/ext3/ext4 filesystems


`-l`   -->     `list` `attributes` of a file system

```bash
[bob@centos-host ~]$ sudo tune2fs -l /dev/vdd

tune2fs 1.45.6 (20-Mar-2020)
Filesystem volume name:   <none>
Last mounted on:          <not available>
Filesystem UUID:          0bbbee5d-06fc-4972-b54e-f578ac0e269b
Filesystem magic number:  0xEF53
Filesystem revision #:    1 (dynamic)
Filesystem features:      has_journal ext_attr resize_inode dir_index filetype extent 64bit flex_bg sparse_super large_file huge_file dir_nlink extra_isize metadata_csum
Filesystem flags:         signed_directory_hash 
Default mount options:    user_xattr acl
Filesystem state:         clean
Errors behavior:          Continue
Filesystem OS type:       Linux
Inode count:              500224
Block count:              262144
Reserved block count:     13007
Free blocks:              221192
Free inodes:              500213
First block:              0
Block size:               4096
Fragment size:            4096
Group descriptor size:    64
Reserved GDT blocks:      241
Blocks per group:         17336
Fragments per group:      17336
Inodes per group:         31264
Inode blocks per group:   1954
Flex block group size:    16
Filesystem created:       Sat May 20 18:41:56 2023
Last mount time:          n/a
Last write time:          Sat May 20 18:41:56 2023
Mount count:              0
Maximum mount count:      -1
Last checked:             Sat May 20 18:41:56 2023
Check interval:           0 (<none>)
Lifetime writes:          993 kB
Reserved blocks uid:      0 (user root)
Reserved blocks gid:      0 (group root)
First inode:              11
Inode size:               256
Required extra isize:     32
Desired extra isize:      32
Journal inode:            8
Default directory hash:   half_md4
Directory Hash Seed:      37e59a20-5cd5-4bb5-8506-66071f051bf7
Journal backup:           inode blocks
Checksum type:            crc32c
Checksum:                 0xd5a13ce3
```

________________________________________________________________________________________________


## `tune2fs -L`

`-L`  -->  `set/change label`

```bash
[bob@centos-host ~]$ sudo tune2fs -L "NewLabel" /dev/vdd
```

________________________________________________________________________________________________
