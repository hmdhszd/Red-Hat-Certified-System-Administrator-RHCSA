


ls ---> list files

-l  list

-h  human readable

-a  include hidden files


________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ mkdir -p 1/2/3/4/5
[bob@centos-host ~]$ touch 1/2/3/4/5/file

[bob@centos-host ~]$ pwd
/home/bob

[bob@centos-host ~]$ cd 1/2/3/4/5/
[bob@centos-host 5]$ pwd
/home/bob/1/2/3/4/5

[bob@centos-host 5]$ cd -
/home/bob
```

________________________________________________________________________________________________


cp -r       --->    copy recursive

```bash
[bob@centos-host ~]$ cp 1/ /tmp
cp: -r not specified; omitting directory '1/'

[bob@centos-host ~]$ cp -r 1/ /tmp
```

________________________________________________________________________________________________


```bash
[bob@centos-host ~]$ rm -f 1/2/3/4/5/
rm: cannot remove '1/2/3/4/5/': Is a directory

[bob@centos-host ~]$ rm -rf 1/2/3/4/5/
```

________________________________________________________________________________________________

## `cp -a`

copy with `attributes`:

```bash
cp -a
```

________________________________________________________________________________________________


## ` ls --full-time`

show the `exact time` of the `last modified`

```bash
[bob@centos-host ~]$ ls --full-time important_file 
-rw-r--r-- 1 root root 0 2015-12-18 01:30:09.000000000 +0000 important_file
```

________________________________________________________________________________________________
