

only root and the file owner can change the file permission

```bash
[bob@centos-host ~]$ touch myfile

[bob@centos-host ~]$ ls -ltrh myfile
-rw-rw-r-- 1 bob bob 0 May 13 02:11 myfile
```

________________________________________________________________________________________________


create a group

```bash
[bob@centos-host ~]$ sudo groupadd family
 
[bob@centos-host ~]$ cat /etc/group | grep family
family:x:1001:
```

________________________________________________________________________________________________


change group

we can only change to groups that our user is part of

```bash
[bob@centos-host ~]$ sudo chgrp family myfile

[bob@centos-host ~]$ ls -ltrh myfile
-rw-rw-r-- 1 bob family 0 May 13 02:11 myfile
```

________________________________________________________________________________________________


to see which group the current user belongs to

```bash
[bob@centos-host ~]$ groups
bob
```

________________________________________________________________________________________________


change owner of a file

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

```bash
"-" (hyphen) represents a regular file
"d" represents a directory
"l" represents a symbolic link
"c" represents a character device
"b" represents a block device
"p" represents a named pipe (FIFO)
"s" represents a socket
```

________________________________________________________________________________________________


the permissions are evaluated from left to right


 rwx     rwx     rwx
  |       |       |
owner   group   others



for files:

r: read
w: write
x: execute

for directories:

r: ls my-directory/
w: touch my-directory/my-file
x: cd my-directory/


________________________________________________________________________________________________


change permission of a file

u: user (owner)
g: group
o: other

```bash
[bob@centos-host ~]$ ls -l myfile
---------- 1 bob family 0 May 13 02:11 myfile

[bob@centos-host ~]$ chmod u+rwx,g+rw,o+r myfile

[bob@centos-host ~]$ ls -l myfile
-rwxrw-r-- 1 bob family 0 May 13 02:11 myfile
```

________________________________________________________________________________________________


```bash
[bob@centos-host ~]$ chmod u=r,g=w,o=x myfile

[bob@centos-host ~]$ ls -l myfile
-r---w---x 1 bob family 0 May 13 02:11 myfile
```

________________________________________________________________________________________________


```bash
[bob@centos-host ~]$ chmod o= myfile

[bob@centos-host ~]$ ls -l myfile
-r---w---- 1 bob family 0 May 13 02:11 myfile
```

________________________________________________________________________________________________


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
