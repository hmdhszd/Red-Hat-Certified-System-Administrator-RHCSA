### Configuration of AutoFS to Automatically Mount the `/home` Directory from a Remote NFS Server at Boot Time

Here’s how to configure `autofs` to mount the `/nfs/home` directory from the remote NFS server at boot time, ensuring that it’s accessible to all users on the local system.

#### **Assumptions:**
- NFS Server IP: `192.168.2.26`
- Exported directory on the NFS Server: `/nfs/home`
- NFS Client IP: `192.168.2.27`
- The NFS client will mount the remote `/nfs/home` to the local `/home` directory.

### **Server-Side (NFS Server 192.168.2.26):**

1. **Ensure the NFS export is correctly configured:**

   Add the following line to the `/etc/exports` file:
   ```bash
   /nfs/home 192.168.2.27(rw,sync,no_root_squash)
   ```

2. **Apply the export:**

   Run the following commands:
   ```bash
   sudo exportfs -a
   sudo systemctl restart nfs-server
   ```

### **Client-Side (NFS Client 192.168.2.27):**

#### **Step 1: Install AutoFS and NFS Utilities**
First, make sure AutoFS and NFS utilities are installed:
```bash
sudo yum install autofs nfs-utils -y
```

#### **Step 2: Edit the AutoFS Master Configuration File**
Edit the `/etc/auto.master` file to define a mount point for the `/home` directory. Open the file using a text editor:
```bash
sudo vim /etc/auto.master
```

Add the following line:
```
/home /etc/auto.home
```
This tells `autofs` to look for a file named `/etc/auto.home` to manage the mounts under `/home`.

#### **Step 3: Create the AutoFS Map File**
Next, create the `/etc/auto.home` map file to define the NFS mount:
```bash
sudo vim /etc/auto.home
```

Add the following line to mount the NFS server's `/nfs/home` directory to the client’s `/home` directory:
```
* -rw,sync 192.168.2.26:/nfs/home/&
```

- The `*` wildcard allows all directories under `/nfs/home` to be accessible.
- The `/nfs/home/&` means that any directory under `/nfs/home` will be mounted at `/home/[directory_name]` on the client.
- The `rw,sync` options allow for read and write access with synchronous I/O operations.

#### **Step 4: Start and Enable AutoFS**
Start the AutoFS service and enable it to start at boot:
```bash
sudo systemctl start autofs
sudo systemctl enable autofs
```

#### **Step 5: Verify the Mount**
Once AutoFS is running, you can verify that the `/home` directory is mounted from the remote NFS server by checking the contents of `/home` on the NFS client:
```bash
ls /home
```

AutoFS will automatically mount the NFS directories on access, so when a user attempts to access any directory under `/home`, AutoFS will mount it from the NFS server.

#### **Step 6: Test Reboot (Optional)**
To ensure that the AutoFS mount persists across reboots, reboot the client system:
```bash
sudo reboot
```

After the system restarts, test if the `/home` directory from the NFS server is automatically mounted:
```bash
ls /home
```

---

### Summary:
- The remote `/nfs/home` directory on the NFS server `192.168.2.26` is mounted automatically to the client’s `/home` directory at boot.
- AutoFS dynamically mounts subdirectories as they are accessed, ensuring efficient resource usage.
