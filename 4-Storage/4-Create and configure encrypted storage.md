
# `cryptsetup`

to `encrypt` `data` on `disk` we use Crypt Setup

2 `modes` on encryption:

- `plain`
- `LUKS`



________________________________________________________________________________________________


# `plain`

## `cryptsetup open --type plain`

`open` this device for `read` the encrypt data from it and `write` the encrypted data to it

```bash
[bob@centos-host ~]$ sudo cryptsetup open --type plain /dev/vde secretdisk

Enter passphrase for /dev/vde: 
```

OR



## `cryptsetup --verify-passphrase open --type plain`

`--verify-passphrase`  -->  ask the passphrase twice 

```bash
[bob@centos-host ~]$ sudo cryptsetup --verify-passphrase open --type plain /dev/vde secretdisk

Enter passphrase for /dev/vde: 
Verify passphrase: 
```

________________________________________________________________________________________________





## `cryptsetup status`


```bash
[bob@centos-host ~]$ sudo cryptsetup status secretdisk 

/dev/mapper/secretdisk is active.
  type:    PLAIN
  cipher:  aes-cbc-essiv:sha256
  keysize: 256 bits
  key location: dm-crypt
  device:  /dev/vde
  sector size:  512
  offset:  0 sectors
  size:    2097152 sectors
  mode:    read/write
```

________________________________________________________________________________________________





## `mkfs.xfs`


```bash
[bob@centos-host ~]$ sudo mkfs.xfs /dev/mapper/secretdisk

meta-data=/dev/mapper/secretdisk isize=512    agcount=4, agsize=65536 blks
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



## `mount`


```bash
sudo mount /dev/mapper/secretdisk /mnt
```


```bash
[bob@centos-host ~]$ lsblk

NAME         MAJ:MIN RM SIZE RO TYPE  MOUNTPOINT
vda          253:0    0  11G  0 disk  
└─vda1       253:1    0  10G  0 part  /
vdb          253:16   0   1G  0 disk  
vdc          253:32   0   1G  0 disk  
vdd          253:48   0   1G  0 disk  
vde          253:64   0   1G  0 disk  
└─secretdisk 252:0    0   1G  0 crypt /mnt
```

________________________________________________________________________________________________





## `umount`

```bash
sudo umount /mnt
```

________________________________________________________________________________________________




## `cryptsetup close`


```bash
[bob@centos-host ~]$ sudo cryptsetup close secretdisk
```


________________________________________________________________________________________________



# `luksformat`


for luks type, you should first format the device, then open it:

## `cryptsetup luksFormat`


```bash
[bob@centos-host ~]$ sudo cryptsetup luksFormat /dev/vde

WARNING!
========
This will overwrite data on /dev/vde irrevocably.

Are you sure? (Type 'yes' in capital letters): YES
Enter passphrase for /dev/vde: 
Verify passphrase: 
```

________________________________________________________________________________________________



## `cryptsetup open`

we `don't` need to `specify` the `format` of encryption

```bash
[bob@centos-host ~]$ sudo cryptsetup open /dev/vde secretdisk
Enter passphrase for /dev/vde: 
```





________________________________________________________________________________________________


## `cryptsetup luksChangeKey`


`change` the `key`

```bash
[bob@centos-host ~]$ sudo cryptsetup luksChangeKey /dev/vde
```


________________________________________________________________________________________________



## `cryptsetup status`


```bash
[bob@centos-host ~]$ sudo cryptsetup status secretdisk

/dev/mapper/secretdisk is active.
  type:    LUKS2
  cipher:  aes-xts-plain64
  keysize: 512 bits
  key location: keyring
  device:  /dev/vde
  sector size:  512
  offset:  32768 sectors
  size:    2064384 sectors
  mode:    read/write
```


________________________________________________________________________________________________


## `mkfs.xfs`


```bash
[bob@centos-host ~]$ sudo mkfs.xfs /dev/mapper/mysecretdisk
```


________________________________________________________________________________________________



## `cryptsetup close`


```bash
[bob@centos-host ~]$ sudo cryptsetup close mysecretdisk
```


________________________________________________________________________________________________
