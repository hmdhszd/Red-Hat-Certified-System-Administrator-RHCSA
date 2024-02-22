### Configure ServerA to automatically mount the home directories of users tom and sam from ServerB using NFS.

- The home directories on ServerB are located at "/home/tom" and "/home/sam," with user IDs 1010 and 1020, respectively.

- The mount should be established in the local "/home" directory on ServerA, ensuring read and write permissions, efficient resource usage, and seamless user experience.


________________________________________________________________________________________________


### Configuring Autofs on ServerA to Automatically Mount Home Directories



- 1) Install Required Packages:

```bash
yum update -y
yum install nfs-utils autofs -y
```

________________________________________________________________________________________________

- 2) Create User Accounts on ServerA:

```bash
useradd -M -u 1010 tom

useradd -M -u 1020 sam
```
The -M option prevents local home directory creation, as they'll be mounted from ServerB.

Ensure UIDs match those on ServerB.

________________________________________________________________________________________________


- 3) Configure autofs: `/etc/auto.master`

```bash
/home /etc/auto.home --timeout=60

--timeout=60 unmounts inactive home directories after 60 seconds, conserving resources.
```


Create /etc/auto.home:

* -fstype=nfs,rw,sync ServerB:/home/&

* matches any user's home directory.

ServerB:/home/& specifies the NFS mount source, with & representing the username.


________________________________________________________________________________________________

- 4) Start and Enable autofs:

```bash
systemctl enable --now autofs
```

________________________________________________________________________________________________

- 5) Verification:

Switch to user accounts and verify successful mounting and permissions:

```bash
$ su - tom

$ ls -l /home

$ pwd



$ su - sam

$ ls -l /home

$ pwd
```
