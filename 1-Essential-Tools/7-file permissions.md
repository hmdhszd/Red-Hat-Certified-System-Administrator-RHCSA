
### `Change Permission` : `root` / `owner`

only root and the file owner can change the file permission

```bash
[bob@centos-host ~]$ touch myfile

[bob@centos-host ~]$ ls -ltrh myfile
-rw-rw-r-- 1 bob bob 0 May 13 02:11 myfile
```

________________________________________________________________________________________________

## `groupadd`

create a group

```bash
[bob@centos-host ~]$ sudo groupadd family
 
[bob@centos-host ~]$ cat /etc/group | grep family
family:x:1001:
```

________________________________________________________________________________________________

## `chgrp`

change group

we can only change to groups that our user is part of

```bash
[bob@centos-host ~]$ sudo chgrp family myfile

[bob@centos-host ~]$ ls -ltrh myfile
-rw-rw-r-- 1 bob family 0 May 13 02:11 myfile
```

________________________________________________________________________________________________

## `groups`

to see `current` `group` that user belongs to

```bash
[bob@centos-host ~]$ groups
bob
```

________________________________________________________________________________________________

## `chown` (only root)

`change` `owner` of a file

only root can change the owner of a file

```bash
[bob@centos-host ~]$ sudo chown root myfile

[bob@centos-host ~]$ ls -ltrh myfile
-rw-rw-r-- 1 root family 0 May 13 02:11 myfile
```

________________________________________________________________________________________________


change user and group of a file at the same time

```bash
[bob@centos-host ~]$ sudo chown bob:family myfile

[bob@centos-host ~]$ ls -ltrh myfile
-rw-rw-r-- 1 bob family 0 May 13 02:11 myfile
```

________________________________________________________________________________________________


The first character in this column represents the file type, and it can have one of the following values:

- `-` (hyphen) regular file

- `d` directory

- `l` symbolic link

- `c` character device

- `b` block device

- `p` named pipe (FIFO)

- `s` socket


________________________________________________________________________________________________


the permissions are evaluated from left to right



| User (File Owner) | Group     | Others                |
| :-------- | :------- | :------------------------- |
| `rwx` | `rwx` | `rwx` |




for `files`:

| r | w     | x                |
| :-------- | :------- | :------------------------- |
| `read` | `write` | `execute` |


for `directories`:



| r | w     | x                |
| :-------- | :------- | :------------------------- |
| `ls my-directory/` | `touch my-directory/my-file` | `cd my-directory/` |



________________________________________________________________________________________________

## `chmod` u+rwx,g+rw,o+r

change permission of a file


```bash
[bob@centos-host ~]$ ls -l myfile
---------- 1 bob family 0 May 13 02:11 myfile

[bob@centos-host ~]$ chmod u+rwx,g+rw,o+r myfile

[bob@centos-host ~]$ ls -l myfile
-rwxrw-r-- 1 bob family 0 May 13 02:11 myfile
```

________________________________________________________________________________________________

## `chmod` u=r,g=w,o=x

```bash
[bob@centos-host ~]$ chmod u=r,g=w,o=x myfile

[bob@centos-host ~]$ ls -l myfile
-r---w---x 1 bob family 0 May 13 02:11 myfile
```

________________________________________________________________________________________________

## `chmod` o=

```bash
[bob@centos-host ~]$ chmod o= myfile

[bob@centos-host ~]$ ls -l myfile
-r---w---- 1 bob family 0 May 13 02:11 myfile
```

________________________________________________________________________________________________

## `stat`

0420/-r---w----

```bash
[bob@centos-host ~]$ stat myfile
  File: myfile
  Size: 0               Blocks: 0          IO Block: 4096   regular empty file
Device: 400026h/4194342d        Inode: 4649640     Links: 1
Access: (0420/-r---w----)  Uid: ( 1000/     bob)   Gid: ( 1001/  family)
Access: 2023-05-13 02:11:52.619584805 +0000
Modify: 2023-05-13 02:11:52.619584805 +0000
Change: 2023-05-13 02:49:19.438679888 +0000
 Birth: 2023-05-13 02:11:52.619584805 +0000
```

________________________________________________________________________________________________



## `SUID` (`4`)

when it is set on a file it means that whenever the file is executed, it's going to be executed with the UserID of the owner of the file instead of the UserID of the person who is running that file


________________________________________________________________________________________________


### set SUID

```bash
[bob@centos-host ~]$ chmod u+s myfile
```



________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ touch myfile

[bob@centos-host ~]$ ls -l myfile
-rw-rw-r-- 1 bob bob 0 May 13 11:39 myfile
```


to set it we add `4` in the front of the permission:


```bash
[bob@centos-host ~]$ chmod 4664 myfile

[bob@centos-host ~]$ ls -l myfile
-rwSrw-r-- 1 bob bob 0 May 13 11:39 myfile
```

in this way, everybody is able to execute this file with the permissions of the user bob, instead of his own user


________________________________________________________________________________________________


right now it's in Capital S:

because the owner it has not the permission to execute

```bash
[bob@centos-host ~]$ ls -l myfile
-rwSrw-r-- 1 bob bob 0 May 13 11:39 myfile
```

but if the owner itself has the permission of execute, so it will be lowercase s:

```bash
[bob@centos-host ~]$ ls -l myfile
-rwsrw-r-- 1 bob bob 0 May 13 11:39 myfile
```

________________________________________________________________________________________________




## `GUID` (`2`)

when it is set on a file it means that whenever the file is executed, it's going to be executed with the GroupID of the owner of the file instead of the GroupID of the person who is running that file



________________________________________________________________________________________________


### set GUID

```bash
[bob@centos-host ~]$ chmod g+s myfile
```



________________________________________________________________________________________________



```bash
[bob@centos-host ~]$ touch myNewFile

[bob@centos-host ~]$ ls -l myNewFile 
-rw-rw-r-- 1 bob bob 0 May 13 11:48 myNewFile
```

to set it we add `2` in the front of the permission:


```bash
[bob@centos-host ~]$ chmod 2664 myNewFile

[bob@centos-host ~]$ ls -l myNewFile 
-rw-rwSr-- 1 bob bob 0 May 13 11:48 myNewFile
```


________________________________________________________________________________________________



right now it's in Capital S:

because the group it has not the permission to execute


```bash
[bob@centos-host ~]$ ls -l myNewFile 
-rw-rwSr-- 1 bob bob 0 May 13 11:48 myNewFile
```


```bash
[bob@centos-host ~]$ chmod g+x myNewFile

[bob@centos-host ~]$ ls -l myNewFile 
-rw-rwsr-- 1 bob bob 0 May 13 11:48 myNewFile
```


________________________________________________________________________________________________


## `find . -perm /4000`

### find files with SUID and GUID set

```bash
[bob@centos-host ~]$ find . -perm /4000
./myfile
```

```bash
[bob@centos-host ~]$ find . -perm /2000
./myNewFile
```

________________________________________________________________________________________________


### set both GUID and SUID on a file (`6`)

```bash
[bob@centos-host ~]$ touch fileWithBothPermissions

[bob@centos-host ~]$ chmod 6664 fileWithBothPermissions

[bob@centos-host ~]$ ls -l  fileWithBothPermissions 
-rwSrwSr-- 1 bob bob 0 May 13 11:56 fileWithBothPermissions
```

________________________________________________________________________________________________


## `Sticky bit` (`1`)

it's usually set on directories that is shared between people,

and `only` the `owner` of each file is able to `remove` that file in that directory


________________________________________________________________________________________________


we can set sticky bit in 2 ways:

- chmod o+t stickyDirectory

- chmod 1777 stickyDirectory


________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ ls -ld stickyDirectory/
drwxrwxr-x 2 bob bob 4096 May 13 12:01 stickyDirectory/
```


________________________________________________________________________________________________



```bash
[bob@centos-host ~]$ chmod o+t stickyDirectory/

[bob@centos-host ~]$ ls -ld stickyDirectory/
drwxrwxr-t 2 bob bob 4096 May 13 12:01 stickyDirectory/
```

________________________________________________________________________________________________



```bash
[bob@centos-host ~]$ chmod o-x stickyDirectory/

[bob@centos-host ~]$ ls -ld stickyDirectory/
drwxrwxr-T 2 bob bob 4096 May 13 12:01 stickyDirectory/
```

now it's a capital T !

________________________________________________________________________________________________

### note that the permissions are applied left to right

________________________________________________________________________________________________

`UserID` : `s`       -->     `4`

`GroupID` : `s`      -->     `2`

`StickyBit` : `t`    -->     `1`

________________________________________________________________________________________________



add SUID, GUID, StickyBit to a directory:

```bash
[bob@centos-host ~]$ chmod u+s,g+s,o+t /home/bob/datadir
```

________________________________________________________________________________________________
