To configure a direct map for automounting the NFS share `/common` from `server2` using AutoFS, follow these steps:

### Step 1: Install the necessary software

1. **Install AutoFS and NFS packages** if they are not already installed:
   ```bash
   sudo yum install autofs nfs-utils -y   # For CentOS/RHEL
   sudo apt install autofs nfs-common -y  # For Ubuntu/Debian
   ```

### Step 2: Create a local mount point

1. **Create the directory `/autodir`** where the NFS share will be mounted:
   ```bash
   sudo mkdir -p /autodir
   ```

### Step 3: Configure AutoFS maps

1. **Edit the AutoFS master map** file `/etc/auto.master` to add a direct map configuration:
   ```bash
   sudo nano /etc/auto.master
   ```

   Add the following line:
   ```
   /- /etc/auto.direct
   ```

2. **Create the direct map file** `/etc/auto.direct` to specify the NFS mount:
   ```bash
   sudo nano /etc/auto.direct
   ```

   Add the following line to mount the `/common` NFS share to `/autodir`:
   ```
   /autodir -fstype=nfs,rw server2:/common
   ```

   - `/autodir` is the local mount point.
   - `-fstype=nfs,rw` specifies the file system type (NFS) and mount options (`rw` for read/write).
   - `server2:/common` is the NFS share being mounted.

### Step 4: Start and enable AutoFS

1. **Restart the AutoFS service** to apply the new configuration:
   ```bash
   sudo systemctl restart autofs
   ```

2. **Enable AutoFS to start on boot**:
   ```bash
   sudo systemctl enable autofs
   ```

### Step 5: Test the AutoFS setup

1. **Access the NFS mount** by navigating to the `/autodir` directory:
   ```bash
   cd /autodir
   ls
   ```

   The NFS share `/common` from `server2` should be automatically mounted when accessed.

### Summary

- Installed `autofs` and `nfs-utils` (or `nfs-common` on Ubuntu).
- Configured `/etc/auto.master` to use a direct map `/etc/auto.direct`.
- Created `/etc/auto.direct` with the direct NFS mount configuration.
- Restarted and enabled the AutoFS service.

This setup ensures that the NFS share from `server2` is mounted on demand when accessing `/autodir`.
