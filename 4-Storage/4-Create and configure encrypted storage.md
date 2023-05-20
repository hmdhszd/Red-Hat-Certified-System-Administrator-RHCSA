

to encrypt data on disk we use Crypt Setup

2 modes on encryption:

- plane
- lux



________________________________________________________________________________________________


### plain

--verify-passphrase    ask the passphrase twice 

open              open this device for read the encrypt data from it and write the encrypted data to it

```bash
[bob@centos-host ~]$ sudo cryptsetup open --type plain /dev/vde secretdisk

Enter passphrase for /dev/vde: 
```

OR


```bash
[bob@centos-host ~]$ sudo cryptsetup --verify-passphrase open --type plain /dev/vde secretdisk

Enter passphrase for /dev/vde: 
Verify passphrase: 
```

________________________________________________________________________________________________





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





```bash
sudo mount
```

________________________________________________________________________________________________





```bash
sudo umount
```

________________________________________________________________________________________________





```bash
[bob@centos-host ~]$ sudo cryptsetup close secretdisk
```

________________________________________________________________________________________________





```bash

```

________________________________________________________________________________________________



### luksformat

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





```bash
[bob@centos-host ~]$ sudo cryptsetup luksFormat /dev/vde

WARNING!
========
This will overwrite data on /dev/vde irrevocably.

Are you sure? (Type 'yes' in capital letters): YES
Enter passphrase for /dev/vde: 
Verify passphrase: 
[bob@centos-host ~]$ sudo cryptsetup open /dev/vde secretdisk
Enter passphrase for /dev/vde: 
```

________________________________________________________________________________________________
