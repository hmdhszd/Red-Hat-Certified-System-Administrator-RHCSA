### NFS Client-Side Configuration

#### 1. **Install the NFS Client Utilities**

The `nfs-utils` package is required on the client as well, so make sure it’s installed:

```bash
sudo yum install -y nfs-utils
```

#### 2. **Create a Mount Point for the NFS Share**

Create a directory where the NFS share will be mounted:

```bash
sudo mkdir -p /mnt/nfs_client
```

This directory will be used to mount the shared directory from the server.

#### 3. **Mount the NFS Share Temporarily**

Use the `mount` command to mount the NFS share from the server to the client:

```bash
sudo mount 192.168.2.26:/srv/nfs_share /mnt/nfs_client
```

- `192.168.2.26:/srv/nfs_share`: The NFS server's IP and shared directory.
- `/mnt/nfs_client`: The local directory where the NFS share will be mounted.

This mounts the NFS share temporarily. Once the system reboots, the mount will be lost unless it's made persistent.

#### 4. **Make the NFS Mount Persistent (After Reboot)**

To ensure the NFS share is mounted automatically after a reboot, add an entry to the `/etc/fstab` file:

```bash
sudo vi /etc/fstab
```

Add the following line:

```bash
192.168.2.26:/srv/nfs_share   /mnt/nfs_client   nfs   defaults   0 0
```

Explanation:
- `192.168.2.26:/srv/nfs_share`: The NFS server's export.
- `/mnt/nfs_client`: Local mount point.
- `nfs`: Specifies the filesystem type as NFS.
- `defaults`: Default mount options (e.g., `rw`, `hard`, etc.).
- `0 0`: File system dump and check order (not important for NFS in this case).

To verify the `/etc/fstab` configuration, remount all filesystems listed there with:

```bash
sudo mount -a
```

#### 5. **Verifying the NFS Mount**

To confirm that the NFS share is mounted successfully:

- **Check Mounted Filesystems**:

```bash
df -h | grep nfs

192.168.2.26:/srv/nfs_share   17G  4.7G   13G  28% /mnt/nfs_client
```

This should list the NFS mount along with its size, usage, and mount point.

- **Check Active Mounts**:

```bash
mount | grep nfs

192.168.2.26:/srv/nfs_share on /mnt/nfs_client type nfs4 (rw,relatime,vers=4.2,rsize=262144,wsize=262144,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=192.168.2.27,local_lock=none,addr=192.168.2.26)
```

This should show the mounted NFS filesystem.

- **List Exported NFS Shares from the Server**:

You can check what the NFS server is exporting by running:

```bash
[root@NFSClient ~]$ showmount -e 192.168.2.26

Export list for 192.168.2.26:
/srv/nfs_share 192.168.2.0/24
```

This should display the exported directories from the NFS server.

#### 6. **Test the NFS Share**

```bash
[root@NFSClient ~]$ echo "Hello from /mnt/nfs_client in the NFSClient" > /mnt/nfs_client/my-file.txt
```


```bash
[root@NFSServer ~]$ cat /srv/nfs_share/my-file.txt

Hello from /mnt/nfs_client in the NFSClient
```




---
