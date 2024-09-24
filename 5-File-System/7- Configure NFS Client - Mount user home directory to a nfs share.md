# Configure NFSClient to automatically mount the home directories of users tom and sam from NFSServer using NFS.

- The home directories on NFSServer are located at "/home/tom" and "/home/sam," with user IDs 1010 and 1020, respectively.

- The mount should be established in the local "/home" directory on NFSClient, ensuring read and write permissions, efficient resource usage, and seamless user experience.

---

## Server Side Configuration:

(this step is usually done in the exam and you are supposed to config only the Client Side)

Here are all the configurations needed on the server side (NFSServer: 192.168.2.26):

### Step 1: Install NFS utilities
Install the necessary NFS package:
```bash
sudo yum install nfs-utils -y
```

### Step 2: Create home directories for users `tom` and `sam`
You need to create the home directories on the NFSServer and set their ownership according to the provided user IDs.

```bash
sudo mkdir -p /home/tom /home/sam
sudo chown 1010:1010 /home/tom
sudo chown 1020:1020 /home/sam
```

### Step 3: Configure `/etc/exports` to share the directories
Edit the `/etc/exports` file to define which directories you want to share via NFS and who can access them.

```bash
sudo vi /etc/exports
```

Add the following lines to export `/home/tom` and `/home/sam` to the NFSClient (192.168.2.27) with read-write access and root privileges:

```
/home/tom 192.168.2.27(rw,sync,no_root_squash)
/home/sam 192.168.2.27(rw,sync,no_root_squash)
```

Explanation of options:
- `rw`: Read-write access.
- `sync`: Write changes immediately to disk (ensures data integrity).
- `no_root_squash`: Allows the root user on the client to act as root on the server (use with caution).

### Step 4: Export the directories
After modifying `/etc/exports`, export the new shares with the following command:

```bash
sudo exportfs -r
```

This will re-read the `/etc/exports` file and apply the changes.

### Step 5: Start and enable the NFS server
Ensure the NFS server is running and will start automatically at boot:

```bash
sudo systemctl enable nfs-server
sudo systemctl start nfs-server

sudo systemctl enable rpcbind
sudo systemctl start rpcbind
```

### Step 6: Configure firewall to allow NFS traffic
To allow the NFS service through the firewall, run the following commands:

```bash
sudo firewall-cmd --permanent --add-service=nfs
sudo firewall-cmd --permanent --add-service=rpc-bind
sudo firewall-cmd --permanent --add-service=mountd
sudo firewall-cmd --reload
```

At this point, the server is configured and ready for the NFSClient to mount the shared directories.


---


## Client Side Configuration:


Here are all the configurations needed on the client side (NFSClient: 192.168.2.27) to automatically mount the home directories of users `tom` and `sam` from NFSServer.

### Step 1: Install NFS utilities
First, install the required NFS packages:

```bash
sudo yum install nfs-utils -y
```

### Step 2: Create mount points for `tom` and `sam`
Create directories on the client side where the NFS shares will be mounted. These should be under `/home` as per the requirements:

```bash
sudo mkdir -p /home/tom /home/sam
```

### Step 3: Configure `/etc/fstab` for automatic mounting
To ensure the NFS shares are mounted automatically at boot, you need to configure the `/etc/fstab` file.

Edit the `/etc/fstab` file:

```bash
sudo vi /etc/fstab
```

Add the following lines to mount the home directories of `tom` and `sam` from the NFSServer (192.168.2.26):

```
192.168.2.26:/home/tom /home/tom nfs defaults,rw,sync,_netdev 0 0
192.168.2.26:/home/sam /home/sam nfs defaults,rw,sync,_netdev 0 0
```

- `defaults`: Uses default NFS mount options.
- `rw`: Read-write access.
- `sync`: Ensures data writes are synchronized between the client and server.
- `_netdev`: Ensures the network is up before mounting (useful for systems that rely on network-based filesystems).

### Step 4: Mount the NFS shares
After configuring `/etc/fstab`, you can manually mount all the filesystems specified in the file (including the NFS shares):

```bash
sudo mount -a
```

### Step 5: Verify the mounts
Check whether the directories `/home/tom` and `/home/sam` are successfully mounted:

```bash
df -h
```

You should see something similar to:

```
192.168.2.26:/home/tom  ...  /home/tom
192.168.2.26:/home/sam  ...  /home/sam
```

### Step 6: Create users `tom` and `sam` on the NFSClient
Since the directories are being used as home directories for users `tom` and `sam`, ensure that these users exist on the NFSClient with the correct user IDs (UIDs) to match the NFSServer.

```bash
sudo useradd -u 1010 tom
sudo useradd -u 1020 sam
```

This ensures that the users have the correct ownership over their respective home directories when they log in.

### Step 7: Test the setup


on the Client Side:

```bash
[root@NFSClient ~]# su - sam
[sam@NFSClient ~]$ touch /home/sam/testfile
[sam@NFSClient ~]$ exit
```


```bash
[root@NFSClient ~]# su - bob
[sam@NFSClient ~]$ touch /home/bob/testfile
[sam@NFSClient ~]$ exit
```

on the Server Side:

```bash
[root@NFSServer ~]# ll /home/tom/

-rw-r--r--. 1 1010 1010 0 Sep 24 10:25 testfile



[root@NFSServer ~]# ll /home/sam/

-rw-r--r--. 1 1020 1020 0 Sep 24 10:25 testfile
```


---
