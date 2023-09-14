


## `tar -tf`

to `display` the `content` of an archive

```bash
[bob@centos-host ~]$ tar -tf archive.tar
```

-t        --list

-f        --file

________________________________________________________________________________________________

## `tar -cf`

`create` tar `archive` from a file

```bash
[bob@centos-host ~]$ tar -cf archive.tar file1
```

________________________________________________________________________________________________


create tar archive from a directory

```bash
[bob@centos-host ~]$ tar -cf archive.tar Pictures/
```

________________________________________________________________________________________________


## `tar -rf`

`-r`      `--append`

`append`/`add` a file to an existing tar archive

```bash
[bob@centos-host ~]$ tar -rf archive.tar file1
```

________________________________________________________________________________________________

## `tar -xf`

`extrant` archive

```bash
[bob@centos-host ~]$ tar -xf archive.tar
 ```

________________________________________________________________________________________________


extrant archive to other directory

`-C`    -->   `--directory`

```bash
[bob@centos-host ~]$ tar -xf archive.tar --directory /tmp/
```


________________________________________________________________________________________________
