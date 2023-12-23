


# login as root user

## `sudo --login`

```bash
[bob@centos-host ~]$ sudo --login
```

OR

## `sudo -i`

```bash
[bob@centos-host ~]$ sudo -i
```

________________________________________________________________________________________________


# `su`   -->   switch user

if the user does not have the SUDO priviledges, but knows the root password:


## `su -`

```bash
[bob@centos-host ~]$ su -
Password:
```


OR

## `su -l`

```bash
[bob@centos-host ~]$ su -l
Password:
```


OR

## `su --login`

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



## `passwd --unlock`

unlock the root user


```bash
[bob@centos-host ~]$ sudo passwd --unlock root

Unlocking password for user root.
passwd: Success
```


## `passwd -u`

```bash
[bob@centos-host ~]$ sudo passwd -u root
```


```bash
[bob@centos-host ~]$ sudo passwd -S root
root PS 2023-07-04 0 99999 7 -1 (Password set, SHA512 crypt.)
```
________________________________________________________________________________________________



# prevent users to `login` as `root user`

## `passwd --lock`

lock root user

```bash
[bob@centos-host ~]$ sudo passwd --lock root

Locking password for user root.
passwd: Success
```

```bash
[bob@centos-host ~]$ sudo passwd -S root

root LK 2023-12-23 0 99999 7 -1 (Password locked.)
```

when we lock the user, it cannot login with password, but can login with ssh key
 

________________________________________________________________________________________________


# make sure you only lock root, when your user can use sudo command!

### with neither sudo nor root login, you can do nothing!



________________________________________________________________________________________________
