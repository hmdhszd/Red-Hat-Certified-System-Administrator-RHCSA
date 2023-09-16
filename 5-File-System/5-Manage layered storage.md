


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



mount

```bash
[bob@centos-host ~]$ sudo mkdir /mnt/stratis
```


change the fstab

(the options should be memorized)

```bash
/dev/stratis/my-pool/myfs1 /mnt/stratis xfs x-systemd.requires=stratisd.service 0 0
```

then

```bash
mount -a
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
