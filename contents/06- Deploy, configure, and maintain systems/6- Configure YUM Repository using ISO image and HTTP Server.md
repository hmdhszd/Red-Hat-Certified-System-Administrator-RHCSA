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

To avoid it, edit `/etc/yum/pluginconf.d/subscription-manager.conf` and set `enabled=0`.

________________________________________________________________________________________________


7. Verify the repository setup:

```bash
yum repolist

yum update
```


________________________________________________________________________________________________

# Configure a Local Yum/DNF Repository on a Server using an HTTP Server.



1. Disable the other repositories:

```bash
mv /etc/yum.repos.d/*.repo /tmp/

yum clean all
```

2. Create a repository file `/etc/yum.repos.d/rhel9.repo` :


```bash
[LocalRepo_BaseOS]

name=LocalRepo_BaseOS

enabled=1

gpgcheck=0

baseurl=http://192.168.1.12/rhel9_repo/BaseOS



[LocalRepo_AppStream]

name=LocalRepo_AppStream

enabled=1

gpgcheck=0

baseurl=http://192.168.1.12/rhel9_repo/AppStream/
```




3. Set file permissions `644` :

```bash
chmod 644 /etc/yum.repos.d/rhel9.repo
```


4. Verify the repository setup:

```bash
yum repolist

yum update
```


________________________________________________________________________________________________



# Configure a Local Yum/DNF Repository on a Server using a RHEL 9 DVD



1. Disable the other repositories:

```bash
mv /etc/yum.repos.d/*.repo /tmp/

yum clean all
```

________________________________________________________________________________________________


2. Create a new directory “/mnt/”:

```bash
mkdir -p /mnt/
```

________________________________________________________________________________________________



3. Mount the RHEL-9 ISO image:

```bash
mount -o loop RHEL-9.iso /mnt
```
________________________________________________________________________________________________

4. make the mount persistent :


```bash
echo "/dev/sr0 /mnt/repo/ iso9660 ro,user,auto 0 0" >> /etc/fstab

mount -a
```

________________________________________________________________________________________________

5. Create the local repository file:


```bash
cp /mnt/media.repo /etc/yum.repos.d/rhel9.repo
```



________________________________________________________________________________________________

6. Set file permissions `644` :

```bash
chmod 644 /etc/yum.repos.d/rhel9.repo
```
________________________________________________________________________________________________

7. Edit the repository file `/etc/yum.repos.d/rhel9.repo` :


```bash
[InstallMedia]

name=DVD for Red Hat Enterprise Linux 9.1 Server

mediaid=1359576196.686790

metadata_expire=-1

gpgcheck=0

cost=500

enabled=1

baseurl=file:///mnt/repo/
```

________________________________________________________________________________________________


8. Clean system caches :

```bash
yum clean all

subscription-manager clean
```

Note: You might see a "This system is not registered" message.

To avoid it, edit /etc/yum/pluginconf.d/subscription-manager.conf and set enabled=0.

________________________________________________________________________________________________


9. Verify the repository setup:

```bash
yum repolist

yum update
```


________________________________________________________________________________________________











