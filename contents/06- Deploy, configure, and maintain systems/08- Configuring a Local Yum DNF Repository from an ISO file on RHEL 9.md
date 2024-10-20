### **RHCSA Exam Topic: Configure a Local Yum/DNF Repository on a Server from an ISO File (RHEL 9)**

In the **RHCSA exam**, one task you may encounter is to configure a **local Yum/DNF repository** using an ISO file. This is useful when your system doesn't have internet access, and you need to install software packages from a local source. In this case, you would mount an ISO file, make the mount persistent, and configure the repository to pull packages from the ISO.

---

### **Key Concepts**

- **ISO File**: An ISO is a disk image file containing an exact copy of a filesystem, typically used for installation media like the RHEL DVD.
  
- **Yum/DNF Repository**: A repository provides packages and metadata for Yum/DNF to use. Configuring a local repository allows you to install and manage software without accessing the internet.

- **Persistence**: Ensuring the mount and repository configuration persist across reboots by properly configuring `/etc/fstab` and saving the repository configuration in `/etc/yum.repos.d/`.

---

### **Steps for Configuring a Local Yum/DNF Repository from an ISO on RHEL 9**

---

#### **1. Mount the ISO File in a Persistent Way**

To set up the ISO as a repository, you must mount it persistently, so it stays mounted after reboot.

1. **Copy the ISO file** to your server or ensure it’s accessible. For example, place it in `/root/iso/rhel9.iso`.

2. **Create a directory** where you will mount the ISO file. For example, `/mnt/iso`:

   ```bash
   mkdir -p /mnt/iso
   ```

3. **Mount the ISO persistently** by adding it to `/etc/fstab`. First, open the `/etc/fstab` file in an editor:

   ```bash
   nano /etc/fstab
   ```

4. **Add an entry** for the ISO file to the `/etc/fstab` file:

   ```
   /root/iso/rhel9.iso  /mnt/iso  iso9660  loop  0  0
   ```

   This tells the system to mount the ISO file at `/mnt/iso` automatically during boot.

5. **Mount the ISO immediately** using:

   ```bash
   mount -a
   ```

---

#### **2. Configure the DNF/Yum Repository**

Now that the ISO is mounted, you need to configure DNF to use it as a repository.

1. **Use `dnf config-manager` to add the repository** pointing to the mounted ISO:

   ```bash
   dnf config-manager --add-repo=file:///mnt/iso
   ```

   This command will automatically create a `.repo` file under `/etc/yum.repos.d/`.

2. **Disable the GPG check** to avoid issues when installing packages from this ISO. Open the `.repo` file that was created (it will have a name like `mnt_iso.repo`):

   ```bash
   nano /etc/yum.repos.d/mnt_iso.repo
   ```

3. **Modify the repository configuration** by adding or updating the following lines to disable GPG checking:

   ```bash
   [mnt-iso]
   name=RHEL 9 ISO Repo
   baseurl=file:///mnt/iso
   enabled=1
   gpgcheck=0
   ```

   This configuration will make the repository functional without requiring GPG checks.

---

#### **3. Verify the Repository**

1. **Clean the DNF cache** to refresh repository metadata:

   ```bash
   dnf clean all
   ```

2. **List the repositories** to ensure the ISO-based repository is enabled:

   ```bash
   dnf repolist
   ```

   You should see output like this:

   ```
   repo id           repo name                   status
   mnt-iso           RHEL 9 ISO Repo             enabled
   ```

3. **Test the repository** by installing a package from the ISO, such as `vim`:

   ```bash
   dnf install vim -y
   ```

   The package should be installed from the ISO file.

---


### **Summary**

For the **RHCSA exam**, knowing how to **configure a local Yum/DNF repository from an ISO file** is important. The key steps are:

1. **Mount the ISO persistently** by editing `/etc/fstab`.
2. **Configure the repository** using `dnf config-manager`.
3. **Disable GPG checking** to avoid errors when installing packages.
4. **Test the setup** to ensure the repository works.

By following this process, you ensure that your system can access the ISO-based repository even after reboot, and that the repository configuration is persistent.

This task is commonly asked in the RHCSA exam to test your ability to work with local resources and repositories.
