


### login as root user



```bash
[bob@centos-host ~]$ sudo --login
```

OR

```bash
[bob@centos-host ~]$ sudo -i
```

________________________________________________________________________________________________


### su      switch user

if the user does not have the SUDO priviledges, but knows the root password:


```bash
[bob@centos-host ~]$ su -
Password:
```


OR

```bash
[bob@centos-host ~]$ su -l
Password:
```


OR

```bash
[bob@centos-host ~]$ su --login
Password:
```

________________________________________________________________________________________________



set password for root in order to login

```bash
[bob@centos-host ~]$ sudo passwd root
```

________________________________________________________________________________________________



unlock the root user


```bash
[bob@centos-host ~]$ sudo passwd --unlock root
```

```bash
[bob@centos-host ~]$ sudo passwd -u root
```


________________________________________________________________________________________________



### prevent users to login as root user

lock root user

```bash
[bob@centos-host ~]$ sudo passwd --lock root
```

when we lock the user, it cannot login with password, but can login with ssh key
 

________________________________________________________________________________________________


# make sure you only lock root, when your user can use sudo command!

### with neither sudo nor root login, you can do nothing!

```bash

```

________________________________________________________________________________________________
