

### file access list


add permission to user

```bash
[bob@centos-host ~]$ sudo setfacl --modify user:john:rw specialfile 
```

________________________________________________________________________________________________



```bash
[bob@centos-host ~]$ ls -l specialfile

-rw-rw-r--+ 1 bob bob 0 May 20 15:51 specialfile
```

________________________________________________________________________________________________


remove permission


```bash
[bob@centos-host ~]$ sudo setfacl --remove user:john specialfile 
```

________________________________________________________________________________________________


add permission to group

```bash
[bob@centos-host ~]$ sudo setfacl --modify group:mail:rx specialfile 
```

________________________________________________________________________________________________


add permission to a directory recursively

```bash
[bob@centos-host ~]$ tree collection/

collection/
├── file1
├── file2
└── file3

0 directories, 3 files
```



```bash
[bob@centos-host ~]$ sudo setfacl --recursive --modify user:john:rwx collection/
```


________________________________________________________________________________________________


deny all permissions from a user on a file

```bash
[bob@centos-host ~]$ sudo setfacl --modify user:hamid:--- myfile
```

________________________________________________________________________________________________


remove facl of a user or a group from a file

```bash
[bob@centos-host ~]$ sudo setfacl --remove user:hamid myfile
```


```bash
[bob@centos-host ~]$ sudo setfacl --remove group:wheel myfile
```

________________________________________________________________________________________________





mask     -->     maximum permission that this file or directory can have




```bash
[bob@centos-host ~]$ sudo setfacl --modify mask:r myfile
```



effective   -->     the actual effective permission

```bash
getfacl myfile
```

________________________________________________________________________________________________


# chattr - change file attributes on a Linux file system

### make immutable:

```bash
[bob@centos-host ~]$ sudo chattr +i specialfile
```

________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ sudo rm specialfile

rm: cannot remove 'specialfile': Operation not permitted
```

________________________________________________________________________________________________


remove immutable attribute

```bash
[bob@centos-host ~]$ sudo chattr -i specialfile
```

________________________________________________________________________________________________

### append (add data at the end of the file without editing that)

```bash
[bob@centos-host ~]$ sudo chattr +a specialfile
```

________________________________________________________________________________________________


### show the attributes of a file

```bash
[bob@centos-host ~]$ sudo lsattr specialfile
```

________________________________________________________________________________________________
