### Steps to Configure the NFS Server:

#### 1. **Install Required NFS Packages**

First, install the NFS utilities package on the server:

```bash
sudo yum install -y nfs-utils
```

#### 2. **Start and Enable NFS Services**

Enable and start the necessary services for NFS:

```bash
sudo systemctl enable nfs-server
sudo systemctl start nfs-server

sudo systemctl enable rpcbind
sudo systemctl start rpcbind
```

#### 3. **Create a Directory to Share**

Create the directory you want to share and set its permissions:

```bash
sudo mkdir -p /srv/nfs_share
sudo chown nobody:nobody /srv/nfs_share
sudo chmod 755 /srv/nfs_share
```

The `nfsnobody` user is used for shared directories by default for security reasons.

#### 4. **Configure the NFS Exports**

Edit the `/etc/exports` file to define which directory to share and the permissions for the clients:

```bash
sudo vi /etc/exports
```

Add a line similar to the following, depending on your network setup and access needs:

```bash
/srv/nfs_share    192.168.2.0/24(rw,sync,no_root_squash)
```

- `/srv/nfs_share`: The directory to be shared.
- `192.168.2.0/24`: The network or IP address of clients that can access the share.
- `rw`: Grants read and write access.
- `sync`: Forces synchronous writes for data integrity.
- `no_root_squash`: Allows the root user on the client machine to have root privileges on the NFS share (for testing purposes; use with caution in production).

#### 5. **Export the NFS Share**

After configuring `/etc/exports`, apply the changes:

```bash
sudo exportfs -r
```

#### 6. **Adjust SELinux for NFS (If Enabled)**

If SELinux is enforcing, you need to allow NFS exports:

```bash
sudo setsebool -P nfs_export_all_rw 1
```

#### 7. **Configure Firewall Rules for NFS**

To allow NFS traffic through the firewall, add the required services:

```bash
sudo firewall-cmd --permanent --add-service=nfs
sudo firewall-cmd --permanent --add-service=rpc-bind
sudo firewall-cmd --permanent --add-service=mountd
sudo firewall-cmd --reload
```

#### 8. **SELinux Context (if applicable)**

Don't forget to set the correct SELinux context if SELinux is enforcing:

```bash
sudo semanage fcontext -a -t nfs_t "/srv/nfs_share(/.*)?"
sudo restorecon -Rv /srv/nfs_share
```

---

#### 8. **Verification**

```bash
[root@NFSServer ~]# exportfs
/srv/nfs_share  192.168.2.0/24
```
