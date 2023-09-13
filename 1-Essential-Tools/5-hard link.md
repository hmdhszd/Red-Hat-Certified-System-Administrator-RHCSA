

the `inode` of each file remembers where each piece of a file is stored in the disk

and also tracks of `metadata` such as `Permissions`, `Access Time`

a file points to the inode, and an inode points to all blocks of data on the disk

`File`: pictures/dog.jpg      -->       `Inode`: 108178360      -->       blocks of `data` on the `disk`


if one of the hard links deletes, the file still remains, it will be deleted when all of the hard links are removed


```bash
[bob@centos-host ~]$ echo "this is my data" > pictures/dog.jpg
```
## `ls -il`

```bash
[bob@centos-host ~]$ ls -il pictures/dog.jpg 
108178360 -rw-rw-r-- 1 bob bob 16 May 12 23:33 pictures/dog.jpg
```

________________________________________________________________________________________________

## `stat`

```bash
[bob@centos-host ~]$ stat pictures/dog.jpg 
  File: pictures/dog.jpg
  Size: 16              Blocks: 8          IO Block: 4096   regular file
Device: 1c00019h/29360153d      Inode: 108178360   Links: 1
Access: (0664/-rw-rw-r--)  Uid: ( 1000/     bob)   Gid: ( 1000/     bob)
Access: 2023-05-12 23:33:32.342126079 +0000
Modify: 2023-05-12 23:33:32.342126079 +0000
Change: 2023-05-12 23:33:32.342126079 +0000
 Birth: 2023-05-12 23:33:32.342126079 +0000
```

Links: 1      --->      the number of hard links to this inode from files 


________________________________________________________________________________________________


### hard link --> `ln`

### soft link --> `ln -s`

________________________________________________________________________________________________


### create hard link

```bash
[bob@centos-host ~]$ ln pictures/dog.jpg photos/dog.jpg

[bob@centos-host ~]$ ls -li photos/dog.jpg
108178360 -rw-rw-r-- 2 bob bob 16 May 12 23:33 photos/dog.jpg

[bob@centos-host ~]$ ls -li pictures/dog.jpg 
108178360 -rw-rw-r-- 2 bob bob 16 May 12 23:33 pictures/dog.jpg
```


### `same inode`


 pictures/dog.jpg      -->       Inode: 108178360      -->       same blocks of data on the disk
                   
 photos/dog.jpg        -->       Inode: 108178360      -->       same blocks of data on the disk


________________________________________________________________________________________________


Links: 2

```bash
[bob@centos-host ~]$ stat photos/dog.jpg 
  File: photos/dog.jpg
  Size: 16              Blocks: 8          IO Block: 4096   regular file
Device: 1c00019h/29360153d      Inode: 108178360   Links: 2
Access: (0664/-rw-rw-r--)  Uid: ( 1000/     bob)   Gid: ( 1000/     bob)
Access: 2023-05-12 23:33:32.342126079 +0000
Modify: 2023-05-12 23:33:32.342126079 +0000
Change: 2023-05-12 23:48:39.303378641 +0000
 Birth: 2023-05-12 23:33:32.342126079 +0000
```

________________________________________________________________________________________________


### hard link limitations


- `only` can hard link to a `file`, `NOT` a `directory`

- only can hard link to file in the `same` `file system`, (not possible in external hard disk)

- make sure you have `permission` to `create` a hard link in the destination path

- make sure all `users` have the `permission` to have `access` that file



________________________________________________________________________________________________


if you `change` the `permission` on `one` of them, it will be changed on the `other` one as well

```bash
[bob@centos-host ~]$ ls -il pictures/dog.jpg 
108178360 -rw-rw-r-- 2 bob bob 16 May 12 23:33 pictures/dog.jpg

[bob@centos-host ~]$ ls -il photos/dog.jpg 
108178360 -rw-rw-r-- 2 bob bob 16 May 12 23:33 photos/dog.jpg

[bob@centos-host ~]$ chmod 666 photos/dog.jpg

[bob@centos-host ~]$ ls -il pictures/dog.jpg 
108178360 -rw-rw-rw- 2 bob bob 16 May 12 23:33 pictures/dog.jpg

[bob@centos-host ~]$ ls -il photos/dog.jpg 
108178360 -rw-rw-rw- 2 bob bob 16 May 12 23:33 photos/dog.jpg
```

________________________________________________________________________________________________
