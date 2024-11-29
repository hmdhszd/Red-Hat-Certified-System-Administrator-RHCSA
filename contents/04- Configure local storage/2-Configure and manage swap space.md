


## Comparison Between `Swap File` and `Swap Partition`

Space Utilization: Swapfiles are more space-efficient since they only consume as much disk space as necessary and can be easily grown or shrunk.

Conversely, swap partitions may either waste space or be insufficient for memory demands.



________________________________________________________________________________________________

# `Swap Partition`

## `swapon --show`

to see info of the swap partition

```bash
[bob@centos-host ~]$ swapon --show

NAME      TYPE SIZE USED PRIO
/swapfile file   2G 8.8M   -2
```

________________________________________________________________________________________________


## `cfdisk /dev/vdb`

to add swap, first we make a new partition

```bash
[bob@centos-host ~]$ sudo cfdisk /dev/vdb
```

________________________________________________________________________________________________



## `lsblk`


```bash
[bob@centos-host ~]$ lsblk

NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
vda    253:0    0   11G  0 disk 
└─vda1 253:1    0   10G  0 part /
vdb    253:16   0    1G  0 disk 
└─vdb1 253:17   0 1023M  0 part 
```

________________________________________________________________________________________________


## `mkswap /dev/vdb1`

then change the partition to become a swap

```bash
[bob@centos-host ~]$ sudo mkswap /dev/vdb1

Setting up swapspace version 1, size = 1023 MiB (1072668672 bytes)
no label, UUID=c58dd8d2-f61d-48a7-88de-2c6d7076c836
```

`blkid`

```bash
[bob@centos-host ~]$ sudo blkid /dev/vdb2

/dev/vdb2: UUID="d8994ff1-6878-4e69-9fa8-bc0a7b488cd0" TYPE="swap" PARTUUID="c6d9a5d8-02"
```


________________________________________________________________________________________________


## `swapon /dev/vdb1`

finally, add that partition to the machine (Temporary)

```bash
[bob@centos-host ~]$ sudo swapon /dev/vdb1
```

## `swapon --show`


```bash
[bob@centos-host ~]$ sudo swapon --show
 
NAME      TYPE       SIZE USED PRIO
/swapfile file         2G 6.8M   -2
/dev/vdb1 partition 1023M   0B   -3
```

this swap will be used until restart of the machine

if we want to add this swap permanently, we should edit `/etc/fstab`:

```bash
[bob@centos-host ~]$ cat /etc/fstab | grep swap

/dev/vdb2 none swap defaults 0 0

/swapfile none swap defaults 0 0

UUeID="a2551c92-a66e-4b91-a18c-1f98d42b86d1" none swap defaults 0 0
```


```bash
[bob@centos-host ~]$ mount -a
```


```bash
[bob@centos-host ~]$ lsblk

NAME   MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
vda    253:0    0  11G  0 disk 
└─vda1 253:1    0  10G  0 part /
vdb    253:16   0   1G  0 disk 
├─vdb2 253:18   0  21M  0 part [SWAP]
└─vdb3 253:19   0  15M  0 part 
```

________________________________________________________________________________________________


## `swapoff /dev/vdb1`

swapon, swapoff - enable/disable devices and files for paging and swapping

```bash
[bob@centos-host ~]$ sudo swapoff /dev/vdb1
```


## `swapon --show`

```bash
[bob@centos-host ~]$ sudo swapon --show
 
NAME      TYPE       SIZE USED PRIO
/swapfile file         2G 6.8M   -2
```

________________________________________________________________________________________________


# `Swap File`

use file instead for swap of partition


## `dd`

```bash
bob@centos-host ~]$ sudo dd if=/dev/zero of=/swap bs=1M count=128 status=progress

128+0 records in
128+0 records out
134217728 bytes (134 MB, 128 MiB) copied, 0.0729469 s, 1.8 GB/s
```


```bash
[bob@centos-host ~]$ ls -ltrh /swap

-rw-r--r--. 1 root root 128M May 15 01:04 /swap
```

________________________________________________________________________________________________


limit the swap for other users

```bash
[bob@centos-host ~]$ sudo chmod 600 /swap
```

________________________________________________________________________________________________




## `mkswap /swap`

```bash
[bob@centos-host ~]$ sudo mkswap /swap

Setting up swapspace version 1, size = 128 MiB (134213632 bytes)
no label, UUID=4bf780ab-4324-4afd-adfe-892748f4787e
```

## `swapon /swap`

```bash
[bob@centos-host ~]$ sudo swapon /swap
```

## `swapon --show`

```bash
[bob@centos-host ~]$ sudo swapon --show

NAME      TYPE SIZE USED PRIO
/swapfile file   2G 9.8M   -2
/swap     file 128M   0B   -3
```

________________________________________________________________________________________________



## Increase the size of an existing swap file:


```bash
sudo swapoff /swapfile
```

```bash
sudo dd if=/dev/zero of=/swapfile bs=1M count=1024 oflag=append conv=notrunc
```

```bash
sudo mkswap /swapfile
```

```bash
sudo swapon /swapfile
```

________________________________________________________________________________________________


To create two swap areas as described, one on a partition (`/dev/sdc3`) and another on a logical volume (`/dev/vgfs/swapvol`), and add them to the `/etc/fstab` for persistence, follow these steps:

### 1. Create and configure the swap partition on `/dev/sdc3`:

#### a. Create the partition:
If the partition `/dev/sdc3` does not already exist, use `fdisk` or `parted` to create it:
```bash
sudo fdisk /dev/sdc
```
In the `fdisk` interactive mode:
- Press `n` to create a new partition.
- Select the partition number (3 in this case).
- Set the size to `+40M`.
- Change the partition type to Linux swap (`82`) using the `t` command.
- Save changes and exit with `w`.

#### b. Set up the swap area:
```bash
sudo mkswap /dev/sdc3
```

#### c. Get the UUID for the partition:
```bash
sudo blkid /dev/sdc3
```
Take note of the UUID from the output.

### 2. Create and configure the swap logical volume (`swapvol` in `vgfs`):

#### a. Create the logical volume:
```bash
sudo lvcreate -L 140M -n swapvol vgfs
```

#### b. Set up the swap area:
```bash
sudo mkswap /dev/vgfs/swapvol
```

### 3. Edit `/etc/fstab` to make the swap areas persistent:

Open `/etc/fstab` in your preferred editor:
```bash
sudo nano /etc/fstab
```

Add the following lines to the file:

```bash
UUID=<uuid-of-sdc3> none swap defaults,pri=1 0 0
/dev/vgfs/swapvol none swap defaults,pri=2 0 0
```

Replace `<uuid-of-sdc3>` with the actual UUID you got from the `blkid` command.

### 4. Activate the swap areas:

```bash
sudo swapon -a
```

### 5. Verify the swap is active:

To check that both swap areas are activated and functioning correctly, use:

```bash
sudo swapon --show
```

You should see both `/dev/sdc3` and `/dev/vgfs/swapvol` listed with their respective priorities.

### 6. Validate swap configuration:

Use `free` or `swapon` to confirm the swap configuration:

```bash
free -h
```

This command shows the total available swap space. You should see the swap size reflecting the two areas you've created.

By following these steps, you will have configured two swap areas (one on the partition and another on the logical volume), made them persistent through the `/etc/fstab` file, and ensured they are active with the appropriate priorities.



