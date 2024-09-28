


## `stratisd` = local storage management tool

it manages pools of physical storages and helps simplify storage management

storage pool = one or more disks or devices or block devices

volumes can be created on top of pools

features:

- Filesystem `Snapshots`

- Thin `Provisioning`

- `Tiering`


________________________________________________________________________________________________


`stratis` is `similar` to `LVM`, but it's designed to be easier to config

it uses `XFS` file system

________________________________________________________________________________________________


install stratis

```bash
sudo yum install stratisd stratis-cli

sudo systemctl enable --now stratisd.service
```

________________________________________________________________________________________________


## `stratis pool create`

create stratis pool

```bash
[bob@centos-host ~]$ sudo stratis pool create my-pool /dev/vdc
```


________________________________________________________________________________________________




## `stratis pool list`

```bash
[bob@centos-host ~]$ stratis pool list

Name                      Total Physical   Properties                                   UUID
my-pool   1 GiB / 37.63 MiB / 986.37 MiB      ~Ca,~Cr   eadb142d-9400-491f-87a6-770850c6e343
```

________________________________________________________________________________________________



## `stratis blockdev`



```bash
[bob@centos-host ~]$ sudo stratis blockdev

Pool Name   Device Node   Physical Size   Tier
my-pool     /dev/vdc              1 GiB   Data
```



```bash
[bob@centos-host ~]$ lsblk

NAME                                                                    MAJ:MIN RM  SIZE RO TYPE    MOUNTPOINT
vda                                                                     253:0    0   11G  0 disk    
└─vda1                                                                  253:1    0   10G  0 part    /
vdb                                                                     253:16   0    1G  0 disk    
vdc                                                                     253:32   0    1G  0 disk    
└─stratis-1-private-c21af14611f44e848f381a057893c978-physical-originsub 252:0    0 1020M  0 stratis 
  ├─stratis-1-private-c21af14611f44e848f381a057893c978-flex-thinmeta    252:1    0   16M  0 stratis 
  │ └─stratis-1-private-c21af14611f44e848f381a057893c978-thinpool-pool  252:4    0  972M  0 stratis 
  ├─stratis-1-private-c21af14611f44e848f381a057893c978-flex-thindata    252:2    0  972M  0 stratis 
  │ └─stratis-1-private-c21af14611f44e848f381a057893c978-thinpool-pool  252:4    0  972M  0 stratis 
  └─stratis-1-private-c21af14611f44e848f381a057893c978-flex-mdv         252:3    0   16M  0 stratis 
vdd                                                                     253:48   0    1G  0 disk    
vde                                                                     253:64   0    1G  0 disk    
```

________________________________________________________________________________________________



## `stratis filesystem create`


create a filesystem on top of stratis pool

```bash
[bob@centos-host ~]$ sudo stratis filesystem create my-pool myfs1
```

________________________________________________________________________________________________




## `stratis fs`



```bash
[bob@centos-host ~]$ stratis fs

Pool Name   Name    Used      Created             Device                       UUID                                
my-pool     myfs1   546 MiB   May 20 2023 22:58   /dev/stratis/my-pool/myfs1   67dcd92c-599a-422c-99e0-b7aba454ad98
```

________________________________________________________________________________________________



## mount

```bash
[bob@centos-host ~]$ sudo mkdir /mnt/stratis
```


change the fstab (`x-systemd.requires=stratisd.service`)

(the options should be memorized)

```bash
/dev/stratis/my-pool/myfs1 /mnt/stratis xfs x-systemd.requires=stratisd.service 0 0
```

then

```bash
mount -a
sudo systemctl daemon-reload
```

```bash
[bob@centos-host ~]$ df -Th | grep stratis

tmpfs                                                                                           tmpfs     1.0M     0  1.0M   0% /run/stratisd/keyfiles
/dev/mapper/stratis-1-39626b1468cb4e5bbc702b68a51d2698-thin-fs-00f5c8f693474b009cbe49e2f55ff26f xfs       1.0T  7.2G 1017G   1% /STRATIS
```

________________________________________________________________________________________________



## `stratis pool add-data`


if you need mode space, you can `add` new `devices` to stratis `pool`

```bash
[bob@centos-host ~]$ sudo stratis pool add-data my-pool /dev/vde
```

________________________________________________________________________________________________




## `stratis pool`

```bash
[bob@centos-host ~]$ stratis pool

Name                     Total Physical   Properties                                   UUID
my-pool   2 GiB / 587.65 MiB / 1.43 GiB      ~Ca,~Cr   eadb142d-9400-491f-87a6-770850c6e343
```

________________________________________________________________________________________________



## `stratis filesystem snapshot`


### save a snapshot of the filesystem

```bash
[bob@centos-host ~]$ sudo stratis filesystem snapshot my-pool myfs1 mypooool-myfs1-snapshot
```

________________________________________________________________________________________________





## `stratis filesystem`


```bash
[bob@centos-host ~]$ sudo stratis filesystem

Pool Name   Name                      Used      Created             Device                                         UUID                                
my-pool     myfs1                     546 MiB   May 20 2023 22:58   /dev/stratis/my-pool/myfs1                     67dcd92c-599a-422c-99e0-b7aba454ad98
my-pool     mypooool-myfs1-snapshot   546 MiB   May 20 2023 23:17   /dev/stratis/my-pool/mypooool-myfs1-snapshot   247c9bdc-e66a-4894-a362-db93755b107b
```

________________________________________________________________________________________________



## `stratis filesystem rename`

recover from snapshot


```bash
[bob@centos-host ~]$ sudo stratis filesystem rename my-pool myfs1 myfs1-old
```





```bash
[bob@centos-host ~]$ sudo stratis filesystem rename my-pool mypooool-myfs1-snapshot myfs1
```

________________________________________________________________________________________________


then unmount and mount again


```bash
[bob@centos-host ~]$ umount /mnt/stratis
```


```bash
[bob@centos-host ~]$ mount /mnt/stratis
```

________________________________________________________________________________________________
