
# LVM VDO


using /dev/sdc, create a deduplicated and compressed logical volume "myvdo" in "myvg" volume group with the following specifications:

- Type "vdo".

- Name "myvdo".

- "myvdo" physical size is 5GiB.

- "myvdo" logical size is 50GiB.

- Create an xfs file system on "myvdo".

- Mount "myvdo" on "/mydir" permanently at boot.


________________________________________________________________________________________________

## 1. Install `lvm2` `vdo` `kmod-kvdo`

```bash
yum install lvm2 vdo kmod-kvdo -y
```

________________________________________________________________________________________________

## 2. Create a partition:

```bash
fdisk /dev/sdc
```

type: 8e //Linux LVM

________________________________________________________________________________________________

## 3. Create a PV:

```bash
pvcreate /dev/sdc1
```
________________________________________________________________________________________________

## 4. Create a VG:

```bash
vgcreate myvg /dev/sdc1
```

________________________________________________________________________________________________

## 5. Create a LV: `lvcreate --name --type vdo --size --virtualsize`

we can create it with specific size:

```bash
lvcreate --name myvdo --type vdo --size 5GiB --virtualsize 50G myvg
```

OR

use all of the free spaces of the Volume Group:

```bash
lvcreate --name myvdo --type vdo --size +100%FREE --virtualsize 50G myvg
```
________________________________________________________________________________________________


## 6. Format the partition (`mkfs.xfs -K`) OR (`mkfs.ext4 -E nodiscard`):

```bash
mkfs.xfs -K /dev/myvg/myvdo
```

OR

```bash
mkfs.ext4 -E nodiscard /dev/myvg/myvdo
```


________________________________________________________________________________________________

## 7. Mount:

```bash
mkdir /mydir
echo /dev/myvg/myvdo /mydir xfs defaults 0 0 >> /etc/fstab
```

```bash
partprobe
mount -a
```




________________________________________________________________________________________________


## `vdostats`

Verify VDO:

```bash
sudo vdostats --human-readable
```


________________________________________________________________________________________________




