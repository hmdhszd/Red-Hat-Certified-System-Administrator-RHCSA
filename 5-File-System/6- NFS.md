# Configure ServerA to automatically mount the home directories of users tom and sam from ServerB using NFS.

- The home directories on ServerB are located at "/home/tom" and "/home/sam," with user IDs 1010 and 1020, respectively.

- The mount should be established in the local "/home" directory on ServerA, ensuring read and write permissions, efficient resource usage, and seamless user experience.


________________________________________________________________________________________________


### Configuring Autofs on ServerA to Automatically Mount Home Directories



### - 1) Install Required Packages:

```bash
yum update -y
yum install nfs-utils autofs -y
```

________________________________________________________________________________________________

### - 2) Create User Accounts on ServerA:

```bash
useradd -M -u 1010 tom

useradd -M -u 1020 sam
```
The -M option prevents local home directory creation, as they'll be mounted from ServerB.

Ensure UIDs match those on ServerB.

________________________________________________________________________________________________


### - 3) Configure autofs: `/etc/auto.master`

```bash
/home /etc/auto.home --timeout=60

--timeout=60 unmounts inactive home directories after 60 seconds, conserving resources.
```


Create /etc/auto.home:

* -fstype=nfs,rw,sync ServerB:/home/&

* matches any user's home directory.

ServerB:/home/& specifies the NFS mount source, with & representing the username.


________________________________________________________________________________________________

### - 4) Start and Enable autofs:

```bash
systemctl enable --now autofs
```

________________________________________________________________________________________________

### - 5) Verification:

Switch to user accounts and verify successful mounting and permissions:

```bash
$ su - tom

$ ls -l /home

$ pwd



$ su - sam

$ ls -l /home

$ pwd
```




________________________________________________________________________________________________


# Configure rhel.server.com (the NFS client) to automatically mount the share rhcsa.server.com:/share on the /nfs directory.


To display the list of NFS exports on the remote system with the hostname “rhcsa.server.com”, run:

```bash
showmount -e rhcsa.server.com
```


```bash
mkdir /nfs
```

```bash
vim /etc/fstab

rhcsa.server.com:/share /nfs nfs _netdev 0 0
```


```bash
mount -a

mount | grep nfs

reboot
```



________________________________________________________________________________________________




# AutoFS

Configure autofs to mount the "/home" directory of the remote NFS server at boot time. Ensure that the mount is accessible to all users on the local system.


- NFS server's: 192.168.1.100

- exported directory: /nfs/home



### - 1) Install `nfs-utils` and `autofs`

```bash
yum install nfs-utils autofs -y
```


### - 2) Edit the `/etc/auto.master` file and add the following line:

```bash
vi /etc/auto.master

/home /etc/auto.nfs
```


### - 3) Create a new file named `/etc/auto.nfs` and add the following line:

```bash
vi /etc/auto.nfs

* -fstype=nfs,rw,soft,intr 192.168.1.100:/nfs/home
```


### - 3) Start the service

```bash
systemctl enable --now autofs
```


________________________________________________________________________________________________


