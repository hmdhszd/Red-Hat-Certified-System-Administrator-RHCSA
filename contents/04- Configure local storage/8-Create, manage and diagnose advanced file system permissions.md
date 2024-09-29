
# File Access List


________________________________________________________________________________________________



## `setfacl --modify user:john:rw specialfile`


`add` permission to `user`

```bash
[bob@centos-host ~]$ sudo setfacl --modify user:john:rw specialfile
```

________________________________________________________________________________________________

when there is a `+` --> file access list is set

```bash
[bob@centos-host ~]$ ls -l specialfile

-rw-rw-r--+ 1 bob bob 0 May 20 15:51 specialfile
```

________________________________________________________________________________________________


## `setfacl --remove user:john specialfile`

`remove` permission from `user`


```bash
[bob@centos-host ~]$ sudo setfacl --remove user:john specialfile
```

________________________________________________________________________________________________



## `setfacl --modify group:mail:rx specialfile`


`add` permission to `group`

```bash
[bob@centos-host ~]$ sudo setfacl --modify group:mail:rx specialfile
```

________________________________________________________________________________________________

## `--recursive`

add permission to a `directory` recursively

```bash
[bob@centos-host ~]$ tree collection/

collection/
├── file1
├── file2
└── file3

0 directories, 3 files
```


## `setfacl --recursive --modify user:john:rwx collection/`



```bash
[bob@centos-host ~]$ sudo setfacl --recursive --modify user:john:rwx collection/
```


________________________________________________________________________________________________


`deny` all permissions from a `user` on a file (`---`)

```bash
[bob@centos-host ~]$ sudo setfacl --modify user:hamid:--- myfile
```

________________________________________________________________________________________________



## `setfacl --remove user:hamid myfile`


`remove` `facl` of a user or a group from a file

```bash
[bob@centos-host ~]$ sudo setfacl --remove user:hamid myfile
```


## `setfacl --remove group:wheel myfile`



```bash
[bob@centos-host ~]$ sudo setfacl --remove group:wheel myfile
```

________________________________________________________________________________________________





## `setfacl --modify mask:r-- myfile`



### `mask`     -->     `maximum` `permission` that this file or directory can have




```bash
[bob@centos-host ~]$ sudo setfacl --modify mask:r-- myfile
```




### `effective`   -->     the `actual` `effective` permission


## `getfacl`

```bash
[bob@centos-host ~]$ ls -l
total 0
-rw-r--r--+ 1 bob bob 0 Sep 23 19:41 archive


[bob@centos-host ~]$ getfacl archive 
# file: archive
# owner: bob
# group: bob
user::rw-
user:john:rwx                   #effective:r--
group::r--
mask::r--
other::r--
```

________________________________________________________________________________________________


# chattr - `change` file `attributes` on a Linux file system

________________________________________________________________________________________________



## `chattr +i specialfile`


###  `add` `immutable` attribute (`+`)

```bash
[bob@centos-host ~]$ sudo chattr +i specialfile
```

________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ sudo rm specialfile

rm: cannot remove 'specialfile': Operation not permitted
```

________________________________________________________________________________________________



## `chattr -i specialfile`


###  `remove` `immutable` attribute (`-`)

```bash
[bob@centos-host ~]$ sudo chattr -i specialfile
```

________________________________________________________________________________________________


## `chattr +a specialfile`


### `append` (add data at the end of the file without editing that)

```bash
[bob@centos-host ~]$ sudo chattr +a specialfile
```

________________________________________________________________________________________________


## `lsattr specialfile`


### show the attributes of a file

```bash
[bob@centos-host ~]$ sudo lsattr specialfile
```

________________________________________________________________________________________________
