### **RHCSA Exam Topic: Configure a Local Yum/DNF Repository on a Server from a HTTP Server (RHEL 9)**

In the **RHCSA exam**, you might be asked to configure a local Yum/DNF repository hosted on an HTTP server. This allows clients to install packages from a central repository via HTTP. In this guide, I’ll explain how to configure both the **server side** and the **client side** so that the changes are persistent.

---

### **Key Concepts**

- **Yum/DNF Repository**: A repository contains packages and metadata that the `dnf` or `yum` command uses to install and update software.
- **HTTP Server**: Used to serve the repository files over a network so that clients can access the packages via HTTP.
- **Persistence**: Ensuring that the HTTP service and the repository configuration are available after a reboot by configuring `/etc/httpd/conf.d/` and `/etc/yum.repos.d/`.

---

### **Steps for Configuring a Local Yum/DNF Repository on a Server from an HTTP Server on RHEL 9**

---

### **Server Side Configuration**

1. **Mount the RHEL ISO Image**

   If you haven’t already, mount the RHEL ISO file persistently on the server. Let’s assume you’ve mounted the ISO to `/mnt/iso` using the following in `/etc/fstab`:

   ```bash
   /root/iso/rhel9.iso  /mnt/iso  iso9660  loop  0  0
   ```

   After modifying `/etc/fstab`, run:

   ```bash
   mount -a
   ```

2. **Install and Configure Apache HTTP Server**

   Install the **httpd** package (Apache HTTP server):

   ```bash
   dnf install httpd -y
   ```

3. **Configure the HTTP Server to Serve the Repository**

   Create a symlink in the web root directory pointing to the mounted ISO directory:

   ```bash
   ln -s /mnt/iso /var/www/html/rhel9-repo
   ```

4. **Modify SELinux to Allow HTTPD Access**

   Allow HTTPD to access files outside the default directory (`/var/www/html`). Enable this with:

   ```bash
   setsebool -P httpd_read_user_content 1
   ```

5. **Start and Enable the HTTP Server**

   Start the Apache HTTP server and enable it to start on boot:

   ```bash
   systemctl start httpd
   systemctl enable httpd
   ```

6. **Verify the HTTP Server**

   You should now be able to access the repository via a web browser or `curl`. Open a browser or use `curl` to check the contents of the repository:

   ```bash
   curl http://<server-ip>/rhel9-repo/
   ```

   You should see the listing of the ISO contents, such as `Packages/`, `repodata/`, etc.

---

### **Client Side Configuration**

1. **Configure the Repository on the Client**

   Use `dnf config-manager` on the client to add the HTTP repository:

   ```bash
   dnf config-manager --add-repo=http://<server-ip>/rhel9-repo
   ```

   This command creates a `.repo` file in `/etc/yum.repos.d/`.

2. **Disable GPG Check to Avoid Errors**

   Open the `.repo` file that was just created (e.g., `/etc/yum.repos.d/<server-ip>_rhel9-repo.repo`), and modify the following settings to disable the GPG check:

   ```bash
   nano /etc/yum.repos.d/<server-ip>_rhel9-repo.repo
   ```

   Add or modify these lines:

   ```bash
   [rhel9-repo]
   name=RHEL 9 HTTP Repo
   baseurl=http://<server-ip>/rhel9-repo/
   enabled=1
   gpgcheck=0
   ```

   Replace `<server-ip>` with the actual IP address of your server.

3. **Clean the DNF Cache and List Repositories**

   Clean the DNF cache and verify the repository is available:

   ```bash
   dnf clean all
   dnf repolist
   ```

   You should see the new repository listed.

4. **Install a Package to Test the Setup**

   Now, try installing a package (e.g., `vim`) from the repository to ensure it works:

   ```bash
   dnf install vim -y
   ```
---

### **Summary**

For the **RHCSA exam**, configuring a **local Yum/DNF repository** from an HTTP server involves the following key steps:

1. **On the server**: Install Apache, serve the repository from the ISO, and configure SELinux to allow access.
2. **On the client**: Add the repository via HTTP, disable GPG checks, and test by installing packages.

The key is making sure all the changes are persistent and properly configured to work across reboots. Using the command `dnf config-manager --add-repo` simplifies the client-side configuration.
