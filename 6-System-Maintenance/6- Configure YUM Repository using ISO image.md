# Configure a Local Yum/DNF Repository on a Server using the RHEL-9 ISO image mounted on the /mnt directory.




________________________________________________________________________________________________




1. Mount the RHEL-9 ISO image:

```bash
mount -o loop RHEL-9.iso /mnt
```
________________________________________________________________________________________________

2. make the mount persistent :


```bash
echo "/path/to/RHEL-9.iso /mnt iso9660 loop 0 0" >> /etc/fstab

mount -a
```

________________________________________________________________________________________________

3. Create the local repository file:


```bash
cp /mnt/media.repo /etc/yum.repos.d/rhel9.repo
```
________________________________________________________________________________________________

4. Set file permissions `644` :

```bash
chmod 644 /etc/yum.repos.d/rhel9.repo
```
________________________________________________________________________________________________

5. Edit the repository file `/etc/yum.repos.d/rhel9.repo` :


```bash
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
```


________________________________________________________________________________________________


6. Clean system caches :

```bash
yum clean all

subscription-manager clean
```

Note: You might see a "This system is not registered" message.

To avoid it, edit /etc/yum/pluginconf.d/subscription-manager.conf and set enabled=0.

________________________________________________________________________________________________


7. Verify the repository setup:

```bash
yum repolist

yum update
```
