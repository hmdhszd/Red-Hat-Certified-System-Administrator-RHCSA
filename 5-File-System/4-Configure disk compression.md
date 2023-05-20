

### VDO   Virtual Data Optimizer

- Zero-Block Elimination
- Deduplication
- Compression

```bash
vdo create --name=vdo_storage --device=/dev/vdb --VdoLogicalSize=1-G
```

the size can be bigger than the real storage size

________________________________________________________________________________________________




```bash
sudo vdostats --human-readable
```

________________________________________________________________________________________________


format the volume

```bash
sudo mkfs.xfs -K /dev/mapper/vdo_storage
```

________________________________________________________________________________________________


to ensure that everything is updated

```bash
udevadm settle
```

________________________________________________________________________________________________




```bash
sudo vdostats --human-readable
```

________________________________________________________________________________________________




```bash
mkdir /mnt/myvdo
```

________________________________________________________________________________________________


add mount at startup

```bash
vi /etc/fstab
```

(should be memorized)

________________________________________________________________________________________________




```bash
mount -a
```

________________________________________________________________________________________________




```bash
df -h /mnt/myvdo
```

________________________________________________________________________________________________


### test the VDO


```bash
head -c 50M dev/urandom > mydata.txt
```

________________________________________________________________________________________________




```bash
mkdir /mnt/myvdo/dir{1..10}
```

________________________________________________________________________________________________




```bash
for i in `seq 1 10`; do sudo mkdir $i && sudo cp myData.txt /mnt/myvdo/dir$i; done;
```

________________________________________________________________________________________________




```bash
df -h /mnt/myvdo
```

________________________________________________________________________________________________




```bash
sudo vdostats --human-readable
```

________________________________________________________________________________________________


### in RHEL 9 , VDO is integrated into LVM

to create a new VDO volume in RHEL9,  you will treat like LVM and add VDO options to it

```bash
pvcreate /dev/vdb
``` 



```bash
vgcreate vdo_volume /dev/vdb
```



```bash
lvcreate --type vdo -n vdo_storage -L 100%FREE -V 10G vdo_volume/vdo_pool1
```


```bash
sudo mkfs.xfs -K /dev/vdo_volume/vdo_storage
```

OR

```bash
sudo mkfs.ext4 -E nodiscard /dev/vdo_volume/vdo_storage
```


________________________________________________________________________________________________


when we use LVM, we do not need special options at fstab for mounting


```bash
mkdir /mnt/myvdo
```



add mount at startup

```bash
vi /etc/fstab
```


________________________________________________________________________________________________




```bash
mount -a
```

________________________________________________________________________________________________




```bash
df -h /mnt/myvdo
```




```bash
man lvmvdo
```

________________________________________________________________________________________________



```bash

```

________________________________________________________________________________________________




```bash

```

________________________________________________________________________________________________

