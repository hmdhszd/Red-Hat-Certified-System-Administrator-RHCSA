

# VDO   Virtual Data Optimizer (Only for RHEL 8)

- `Zero-Block` `Elimination`
- `Deduplication`
- `Compression`

________________________________________________________________________________________________



install VDO

```bash
yum install vdo kmod-kvdo
systemctl enable --now vdo.service
```

________________________________________________________________________________________________

## `vdo create --name= --device= --VdoLogicalSize=`

```bash
vdo create --name=vdo_storage --device=/dev/vdb --VdoLogicalSize=10G
```

the size can be bigger than the real storage size

________________________________________________________________________________________________



## `vdostats --human-readable`


```bash
sudo vdostats --human-readable
```

________________________________________________________________________________________________


## `mkfs.xfs -K`

format the volume

```bash
sudo mkfs.xfs -K /dev/mapper/vdo_storage
```

________________________________________________________________________________________________


## `udevadm settle`

to ensure that everything is updated

```bash
udevadm settle
```

________________________________________________________________________________________________


we create a new directory to mount the vdo storage:

```bash
mkdir /mnt/myvdo
```

________________________________________________________________________________________________


add mount at startup

```bash
vi /etc/fstab

/dev/mapper/vdo_storage        /mnt/myvdo                    xfs
 _netdev,x-systemd.device-timeout=0,x-systemd.requires=vdo.service      0 0
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

put random data into a file

```bash
head -c 50M dev/urandom > mydata.txt
```




```bash
mkdir /mnt/myvdo/dir{1..10}
```




```bash
for i in `seq 1 10`; do sudo mkdir $i && sudo cp myData.txt /mnt/myvdo/dir$i; done;
```

Verify:



```bash
df -h /mnt/myvdo
```

________________________________________________________________________________________________
