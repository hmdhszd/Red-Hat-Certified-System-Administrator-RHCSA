


wheel group     -->     root permission

docker group    -->     manage docker containers 

a user have one primary group, but can be in several other groups

________________________________________________________________________________________________





```bash
[bob@centos-host ~]$ sudo useradd hamid
[bob@centos-host ~]$ sudo groupadd developers
```

________________________________________________________________________________________________


### add a user to a group


```bash
[bob@centos-host ~]$ sudo gpasswd --add hamid developers

Adding user hamid to group developers
```

OR

```bash
[bob@centos-host ~]$ sudo gpasswd -a hamid developers

Adding user hamid to group developers
```


OR

append new secondary group to the list of groups of a user

```bash
[bob@centos-host ~]$ sudo usermod -aG developers hamid
```

OR

change the primary group

```bash
[bob@centos-host ~]$ sudo usermod -g developers hamid
```

________________________________________________________________________________________________





```bash
[bob@centos-host ~]$ groups hamid

hamid : hamid developers
```

________________________________________________________________________________________________




### remove a user from a group




```bash
[bob@centos-host ~]$ sudo gpasswd --delete hamid developers

Removing user hamid from group developers
```

OR


```bash
[bob@centos-host ~]$ sudo gpasswd -r hamid developers

Removing user hamid from group developers
```

________________________________________________________________________________________________


### change the name of a group


```bash
[bob@centos-host ~]$ sudo groupmod --new-name programers developers
```



```bash
[bob@centos-host ~]$ cat /etc/group | grep programers

programers:x:1004:hamid
```

________________________________________________________________________________________________



delete a primary group

```bash
[bob@centos-host ~]$ sudo groupdel programers
```

________________________________________________________________________________________________
