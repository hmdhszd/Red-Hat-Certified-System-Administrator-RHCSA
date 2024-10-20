### **RHCSA Exam Topic: Configure a Local Yum/DNF Repository on a Server from a DVD (RHEL 9)**

In the **RHCSA exam**, you may be asked to configure a **local Yum/DNF repository** using a DVD. This is useful for scenarios where the system doesn't have internet access, and you need to install packages directly from the installation media. To set this up, you must mount the DVD and configure the repository in a **persistent** way so that it remains across reboots.

---

### **Key Concepts**

- **Yum/DNF Repositories**: Yum and DNF are the package managers used to install, update, and manage software packages in RHEL. Repositories contain metadata and packages that these tools use.
  
- **Local Repository**: A repository set up to pull packages from a local filesystem or mounted media like a DVD.

- **DVD as Repository**: The Red Hat installation DVD contains all the necessary packages, which can be used to create a local repository.

- **Repository Configuration**: Repository configuration files are stored under `/etc/yum.repos.d/`.

---

### **Steps for Configuring a Local Yum/DNF Repository from a DVD on RHEL 9**

---

#### **1. Mount the DVD in a Persistent Way**

To configure the DVD repository, first, you need to mount the DVD to the system in a way that it remains mounted after reboots.

1. **Insert the DVD** into your system's drive.

2. **Create a mount point** where the DVD will be mounted, for example `/media/dvd`.
   
   ```bash
   mkdir -p /media/dvd
   ```

3. **Mount the DVD** to this location persistently by adding an entry to the `/etc/fstab` file.

   a. Get the device name of the DVD using `lsblk` or `blkid`. The device name is typically `/dev/sr0`.

   b. Open the `/etc/fstab` file in a text editor:
   
   ```bash
   nano /etc/fstab
   ```

   c. Add the following entry at the bottom of the file:
   
   ```bash
   /dev/sr0    /media/dvd    iso9660    defaults    0  0
   ```

   This ensures the DVD will be automatically mounted at `/media/dvd` during boot.

4. **Mount the DVD now** using the command below:

   ```bash
   mount -a
   ```

   This mounts all filesystems listed in `/etc/fstab`.

---

#### **2. Add the DVD as a Yum/DNF Repository**

You can easily add the DVD as a repository using the `dnf config-manager --add-repo` command.

1. Use the following command to create a repository configuration:

   ```bash
   dnf config-manager --add-repo=file:///media/dvd
   ```

   This command creates a new repository configuration file under `/etc/yum.repos.d/` that points to the mounted DVD at `/media/dvd`.

---

#### **3. Disable GPG Check**

Since you are using a local DVD repository, you may want to disable GPG checking to prevent errors.

1. **Edit the repo file** that was created in `/etc/yum.repos.d/`.

   The file will be named something like `/etc/yum.repos.d/media_dvd.repo`.

2. **Open the file** in a text editor:
   
   ```bash
   nano /etc/yum.repos.d/media_dvd.repo
   ```

3. **Add or modify** the following lines to disable the GPG check:
   
   ```bash
   [dvd-repo]
   name=RHEL 9 DVD Repo
   baseurl=file:///media/dvd
   enabled=1
   gpgcheck=0
   ```

---

#### **4. Verify the Repository**

1. **Clear the DNF cache** to refresh the repository metadata:
   
   ```bash
   dnf clean all
   ```

2. **Check the repository list** to ensure the DVD repo is listed:

   ```bash
   dnf repolist
   ```

   You should see something like:

   ```
   repo id           repo name                   status
   dvd-repo          RHEL 9 DVD Repo             enabled
   ```

3. **Test the repository** by installing a package from the DVD, for example:

   ```bash
   dnf install vim -y
   ```

   This should install the `vim` package from the DVD.

---

### **Summary**

For the **RHCSA exam**, you need to know how to **configure a local Yum/DNF repository from a DVD** and make it **persistent** across reboots. You must:

1. **Mount the DVD persistently** using `/etc/fstab`.
2. **Create a repository** using `dnf config-manager --add-repo`.
3. **Disable GPG checking** to avoid potential issues when installing packages.
4. **Test the repository** by installing a package from the DVD.

By following these steps, you ensure that the system can install packages even without internet access, and the configuration remains after reboots.
