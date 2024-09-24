You can configure the automatic mounting of NFS shares using **autofs** instead of directly configuring them in `/etc/fstab`. 

### The difference between **`/etc/fstab`** and **autofs**:

- **`/etc/fstab`** mounts the NFS shares at boot. If the NFS server is down or the network is not available at boot, it can cause delays or failures during system startup.
- **autofs** mounts the NFS shares **on demand**, meaning it only mounts the share when a user tries to access the directory. This reduces unnecessary resource usage and prevents problems if the server is unavailable during boot.

### Steps to configure with autofs

### Step 1: Install autofs on the **NFSClient**
```bash
sudo yum install nfs-utils autofs -y
```

### Step 2: Configure autofs
The configuration files for autofs are `/etc/auto.master` and the map file `/etc/auto.home` (or another file you define). 

#### Edit the autofs master file:
Open `/etc/auto.master` to define the mount points managed by autofs:
```bash
sudo vi /etc/auto.master
```

Add the following line to the end of the file:
```
/home /etc/auto.home --timeout=600
```

This tells autofs to use `/etc/auto.home` to manage mounts under `/home`.

#### Step 3: Create the autofs map file
Now, create and edit the `/etc/auto.home` file:
```bash
sudo vi /etc/auto.home
```

Add the following lines to map the home directories of `tom` and `sam` from the NFSServer (192.168.2.26):
```
tom  -rw,sync  192.168.2.26:/home/tom
sam  -rw,sync  192.168.2.26:/home/sam
```

Explanation of the fields:
- `tom` and `sam` are the names of the directories under `/home` (e.g., `/home/tom`).
- `-rw,sync`: NFS mount options (read-write and sync).
- `192.168.2.26:/home/tom` and `192.168.2.26:/home/sam`: The paths to the NFS shares on the server.

### Step 4: Start and enable autofs
After configuring autofs, start and enable the service:

```bash
sudo systemctl start autofs
sudo systemctl enable autofs
```


### Step 5: Create users `tom` and `sam` on the NFSClient
Since these directories are home directories for `tom` and `sam`, you need to create the users with the appropriate user IDs (UIDs) on the NFSClient.

```bash
sudo useradd -u 1010 tom
sudo useradd -u 1020 sam
```


### Step 5: Test autofs

on the Client Side:

```bash
[sam@NFSClient ~]$ df -h | grep home
192.168.2.26:/home/tom   17G  4.7G   13G  28% /home/tom
192.168.2.26:/home/sam   17G  4.7G   13G  28% /home/sam
```

```bash
[root@NFSClient ~]# su - tom
[tom@NFSClient ~]$ echo "Hello from tom in NFSClient" > tom_hello.txt
[tom@NFSClient ~]$ exit
```


```bash
[root@NFSClient ~]# su - sam
[sam@NFSClient ~]$ echo "Hello from sam in NFSClient" > sam_hello.txt
[sam@NFSClient ~]$ exit
```

on the Server Side:

```bash
[root@NFSServer ~]# cat /home/sam/sam_hello.txt

Hello from sam in NFSClient


[root@NFSServer ~]# cat /home/tom/tom_hello.txt

Hello from tom in NFSClient
```


---


### Summary of Autofs vs `/etc/fstab`:

| Feature                   | `/etc/fstab`                         | autofs                                |
|---------------------------|---------------------------------------|---------------------------------------|
| **Mounting**               | Mounted at boot (always mounted)      | Mounted on-demand (only when accessed)|
| **Startup Dependency**     | Can delay boot if NFS server is down  | No delay if the NFS server is down    |
| **Unmounting**             | Always mounted unless manually unmounted | Automatically unmounts after a period of inactivity |
| **Resource Usage**         | Higher, as NFS shares are always mounted | More efficient, mounts only when needed |
| **Reliability**            | Issues if NFS server or network is unavailable at boot | More flexible and avoids boot failures|

In conclusion, autofs provides a more flexible and resource-efficient way of mounting NFS shares, especially in environments where you don’t want the shares to be always mounted, or when network connectivity might be an issue.
