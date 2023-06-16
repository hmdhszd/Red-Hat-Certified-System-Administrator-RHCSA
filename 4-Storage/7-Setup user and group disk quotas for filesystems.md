

install quota

```bash
dnf install quota
```

________________________________________________________________________________________________


change /etc/fstab to enable the quota


```bash
nano /etc/fstab

/dev/sdb1 /mybackups     xfs    defaults,usrquota,grpquota  0 2

```

and then reboot


```bash
systemcto reboot 
```

________________________________________________________________________________________________


enforce quota on ext4 filesystem needs 2 more commands:

1- 

```bash
 quotacheck -cug /dev/sdb1
 ```
 
 OR
 
 
```bash
 quotacheck --create-files --user --group /dev/sdb1
 ```

this will create 2 files on the filesystem: aquata.group and aquata.user

in these files, the system keeps the track of how much data the user or group is using


________________________________________________________________________________________________

2- 


```bash
quotaon /mybackups
```

________________________________________________________________________________________________
________________________________________________________________________________________________

allocate 100M to a user


```bash
fallocate --length 100M /mybackups/hamid/100Mfile
```

________________________________________________________________________________________________


### edit the quata of a user

```bash
edquota --user hamid
```

blocks: the allocated size: 102400 --> 100M

soft limits / hard limits

0 = no limits

 

________________________________________________________________________________________________


### see the quata of a user


```bash
quota --user hamid
```

grace time : when we pass the soft limit, we have time for deleting the extra data / or we will de blocked from any additional change


________________________________________________________________________________________________


for limiting the number of files and directories, we can limit the number of inodes

________________________________________________________________________________________________


edit the grace period:

```bash
quota --edit-period
```

________________________________________________________________________________________________


edit the quota of a group:

```bash
edquota --group adm
```


verify the auota of a group:



```bash
quota --group adm
```
________________________________________________________________________________________________
________________________________________________________________________________________________


Edit disk quotas for the user called john. Set a soft limit of 100 megabytes and hard limit of 500 megabytes on /mnt partition.

```bash
[bob@centos-host ~]$ sudo xfs_quota -x -c 'limit bsoft=100m bhard=500m john' /mnt/
```

