


‍`wheel` group     -->     `root` `permission`

docker group    -->     manage docker containers 

a user have one primary group, but can be in several other groups

________________________________________________________________________________________________





```bash
[bob@centos-host ~]$ sudo useradd hamid
[bob@centos-host ~]$ sudo groupadd developers
```

________________________________________________________________________________________________


# add a user to a group


## `gpasswd --add`

```bash
[bob@centos-host ~]$ sudo gpasswd --add hamid developers

Adding user hamid to group developers
```

OR

## `gpasswd -a`

```bash
[bob@centos-host ~]$ sudo gpasswd -a hamid developers

Adding user hamid to group developers
```


OR

## `usermod -aG`

append new `secondary` group to the list of groups of a user

```bash
[bob@centos-host ~]$ sudo usermod -aG developers hamid
```

OR

## `usermod -g`

change the `primary` group

```bash
[bob@centos-host ~]$ sudo usermod -g developers hamid
```

________________________________________________________________________________________________




## `groups`


```bash
[bob@centos-host ~]$ groups hamid

hamid : hamid developers
```

________________________________________________________________________________________________




# `remove` a user from a group



## `gpasswd --delete`


```bash
[bob@centos-host ~]$ sudo gpasswd --delete hamid developers

Removing user hamid from group developers
```

OR

## `gpasswd -r`


```bash
[bob@centos-host ~]$ sudo gpasswd -r hamid developers

Removing user hamid from group developers
```

________________________________________________________________________________________________


## `groupmod --new-name`

### change the name of a group


```bash
[bob@centos-host ~]$ sudo groupmod --new-name programers developers
```



```bash
[bob@centos-host ~]$ cat /etc/group | grep programers

programers:x:1004:hamid
```

________________________________________________________________________________________________



## `groupdel`

delete a primary group

```bash
[bob@centos-host ~]$ sudo groupdel programers
```

________________________________________________________________________________________________




### Create the following users and groups, configure their permissions for specific directories, and ensure appropriate file ownership for newly created files.



#### Users:

- amr and biko (members of the "admins" group)

- carlos and david (members of the "developers" group)



```bash
groupadd admins

groupadd developers
```

```bash
useradd amr
usermod -aG admins amr
```

```bash
useradd biko
usermod -aG admins biko
```

```bash
useradd carlos
usermod -aG developers carlos
```

```bash
useradd david
usermod -aG developers david
```




#### Directories:

- /admins (accessible only to owner and admins group members, owned by biko)

- /developers (accessible only to developers group members, owned by carlos)


```bash
mkdir /admins

mkdir /developers
```

```bash
chown biko:admins /admins

chmod 770 /admins # Owner and group full access, no access for others
```


```bash
chown carlos:developers /developers

chmod 770 /developers # Owner and group full access, no access for others
```




#### File Ownership:

- New files in /developers or /admins should be owned by the respective group owner. (setgid)

- Only file creators should be allowed to delete their files. (sticky bit)



```bash
chmod +t,g+s /admins # Set sticky bit and setgid bit
```



```bash
chmod +t,g+s /developers # Set sticky bit and setgid bit
```




________________________________________________________________________________________________




