To configure Autofs for automatically mounting the NFS share `/srv/nfs_share` from `NFSServer` (192.168.2.26) to `/mnt/nfs_client` on `NFSClient`, follow these steps. This approach will automatically handle the mounting when the directory is accessed.

### **Client-side Configuration using Autofs**

1. **Install necessary packages**:
   You need to install `autofs` and `nfs-utils` to enable NFS and Autofs support.
   ```bash
   sudo yum install autofs nfs-utils -y
   ```

2. **Configure the Autofs master map (`/etc/auto.master`)**:
   Edit the `/etc/auto.master` file to configure Autofs to manage `/mnt/nfs_client` using a custom map.

   ```bash
   sudo vi /etc/auto.master
   ```

   Add this line:
   ```
   /mnt /etc/auto.nfs --timeout=600
   ```

   This tells Autofs to manage the `/mnt` directory using the configuration defined in `/etc/auto.nfs`, with a timeout of 600 seconds (10 minutes) for automatically unmounting the share after it's idle.

3. **Create and configure the map file (`/etc/auto.nfs`)**:
   This file will specify how the NFS share should be mounted dynamically.

   ```bash
   sudo vi /etc/auto.nfs
   ```

   Add the following line:
   ```
   nfs_client -rw,sync 192.168.2.26:/srv/nfs_share
   ```

   This line specifies that when you access `/mnt/nfs_client`, Autofs will mount the NFS share located at `192.168.2.26:/srv/nfs_share` using read-write (`rw`) and synchronized (`sync`) options.

4. **Start and enable Autofs**:
   After configuring Autofs, start the service and enable it to ensure it runs on boot:

   ```bash
   sudo systemctl start autofs
   sudo systemctl enable autofs
   ```

5. **Create the mount point directory (optional)**:
   Autofs will dynamically manage the directory, so you don't technically need to create `/mnt/nfs_client` manually. However, if you want to verify it:

   ```bash
   sudo mkdir -p /mnt/nfs_client
   ```

   (This directory will be created and mounted automatically by Autofs when accessed.)

6. **Verify the mount**:

   on the client side:

   To ensure that the share is mounted dynamically by Autofs, check the output of `df -h`:
   
   ```bash
   df -h | grep nfs
   
   192.168.2.26:/srv/nfs_share   17G  4.7G   13G  28% /mnt/nfs_client
   ```

   ```bash
   [root@NFSClient ~]# echo "Hello from /mnt/nfs_client/myfile.txt on the NFS Client" > /mnt/nfs_client/myfile.txt
   ```
    
   ```bash
   [root@NFSServer ~]# cat /srv/nfs_share/myfile.txt
   
   Hello from /mnt/nfs_client/myfile.txt on the NFS Client
   ```


---

