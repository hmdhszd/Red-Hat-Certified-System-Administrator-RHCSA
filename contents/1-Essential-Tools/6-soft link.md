

soft link is like a text file, inside of that you can find the path to the file or directory


`soft-link`  -->  `File`: pictures/dog.jpg  -->  `Inode`: 108178360  -->  blocks of `data` on the `disk`

________________________________________________________________________________________________

### create soft link (`ln -s`)

ln -s target-file soft-link-file

```bash
[bob@centos-host ~]$ ls -il pictures/dog.jpg
116895763 -rw-rw-r-- 1 bob bob 0 May 13 00:46 pictures/dog.jpg

[bob@centos-host ~]$ ln -s pictures/dog.jpg photos/dog.jpg

[bob@centos-host ~]$ ls -il photos/dog.jpg 
116895764 lrwxrwxrwx 1 bob bob 16 May 13 00:47 photos/dog.jpg -> pictures/dog.jpg
```

________________________________________________________________________________________________

## `readlink`

use readlink command to see where a file is linked to

```bash
[bob@centos-host ~]$ readlink photos/dog.jpg 
pictures/dog.jpg
```

________________________________________________________________________________________________

### soft link limitations

the `permission` of the soft link do not matter, so it's `lrwxrwxrwx`

the permission of the target file is important

the `inode` is `different` in soft link

you can use soft link for the `directories` and also in `other file systems`


________________________________________________________________________________________________
