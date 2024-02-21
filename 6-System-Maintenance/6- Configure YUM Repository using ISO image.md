# Configure a Local Yum/DNF Repository on a Server using the RHEL-9 ISO image mounted on the /mnt directory.




________________________________________________________________________________________________




1. Mount the RHEL-9 ISO image:

# mount -o loop RHEL-9.iso /mnt

________________________________________________________________________________________________

2. make the mount persistent :

# echo "/path/to/RHEL-9.iso /mnt iso9660 loop 0 0" >> /etc/fstab

________________________________________________________________________________________________

3. Create the local repository file:

Copy the /mnt/media.repo file to /etc/yum.repos.d/rhel9.repo:

```bash
# cp /mnt/media.repo /etc/yum.repos.d/rhel9.repo
```

4. Set file permissions:

Set permissions for /etc/yum.repos.d/rhel9.repo to allow reading by all:

```bash
# chmod 644 /etc/yum.repos.d/rhel9.repo
```

5. Edit the repository file:

Open /etc/yum.repos.d/rhel9.repo in a text editor (e.g., vim).

Replace the existing content with the following:

[InstallMedia-BaseOS]

name=RHEL 9 - BaseOS

metadata_expire=-1

gpgcheck=0

enabled=1

baseurl=file:///mnt/BaseOS/



[InstallMedia-AppStream]

name=RHEL 9 - AppStream

metadata_expire=-1

gpgcheck=0

enabled=1

baseurl=file:///mnt/AppStream/



Explanation:

The file defines two repositories: BaseOS and AppStream.

metadata_expire=-1 disables metadata expiration checks.

gpgcheck=0 skips GPG key verification (can be enabled later).

enabled=1 activates the repository.

baseurl points to the repository base directories within the mounted ISO.

6. Save and quit the editor.

7. Clean system caches (optional):

Clear the Yum/DNF and subscription-manager cache:

# dnf clean all

# subscription-manager clean

Note: You might see a "This system is not registered" message. To avoid it, edit /etc/yum/pluginconf.d/subscription-manager.conf and set enabled=0.

8. Verify the repository setup:

List available repositories:

# dnf repolist

