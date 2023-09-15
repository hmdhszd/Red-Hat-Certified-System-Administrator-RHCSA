


```bash
[bob@centos-host ~]$ ls -ltrh

-rw-rw-r-- 1 bob bob 121K May 13 19:37 file
-rw-rw-r-- 1 bob bob 121K May 13 19:37 file1
-rw-rw-r-- 1 bob bob 121K May 13 19:37 file2
-rw-rw-r-- 1 bob bob 121K May 13 19:37 file3
```

________________________________________________________________________________________________

## `gzip`, `bzip2`, `xz`

```bash
[bob@centos-host ~]$ gzip file1
[bob@centos-host ~]$ bzip2 file2
[bob@centos-host ~]$ xz file3
```

```bash
[bob@centos-host ~]$ ls -ltrh

-rw-rw-r-- 1 bob bob 121K May 13 19:37 file
-rw-rw-r-- 1 bob bob 5.5K May 13 19:37 file1.gz
-rw-rw-r-- 1 bob bob 6.1K May 13 19:37 file2.bz2
-rw-rw-r-- 1 bob bob 4.1K May 13 19:37 file3.xz
```

________________________________________________________________________________________________


## `gunzip`, `bunzip2`, `unxz`


```bash
[bob@centos-host ~]$ gunzip file1
[bob@centos-host ~]$ bunzip2 file2.bz2 
[bob@centos-host ~]$ unxz file3.xz 
```

OR

## `--decompress`

```bash
[bob@centos-host ~]$ gzip --decompress file1
[bob@centos-host ~]$ bzip2 --decompress file2.bz2 
[bob@centos-host ~]$ xz --decompress file3.xz 
```



```bash
[bob@centos-host ~]$ ls -ltrh

-rw-rw-r-- 1 bob bob 121K May 13 19:37 file
-rw-rw-r-- 1 bob bob 121K May 13 19:37 file1
-rw-rw-r-- 1 bob bob 121K May 13 19:37 file2
-rw-rw-r-- 1 bob bob 121K May 13 19:37 file3
```

________________________________________________________________________________________________



## `gunzip --keep`, `bunzip2 --keep`, `unxz --keep`


#### by default they remove the original file after zip the file, to keep the original file we can use `-k` OR `--keep` 


```bash
[bob@centos-host ~]$ gzip --keep file1
[bob@centos-host ~]$ bzip2 --keep file2
[bob@centos-host ~]$ xz --keep file3
```

________________________________________________________________________________________________

## `zip`

#### only zip compress `directory`

```bash
[bob@centos-host ~]$ zip -r archive.zip Pictures/
  adding: Pictures/ (stored 0%)
  adding: Pictures/file2 (stored 0%)
  adding: Pictures/file4 (stored 0%)
  adding: Pictures/file5 (stored 0%)
  adding: Pictures/file1 (stored 0%)
  adding: Pictures/file3 (stored 0%)
```

________________________________________________________________________________________________

## `unzip`

```bash
[bob@centos-host ~]$ unzip archive.zip 
```

________________________________________________________________________________________________


### compress with `tar`



- `-z`        `--gzip`

- `-j`        `--bzip2`

- `-J`        `--zx`




```bash
[bob@centos-host ~]$ tar czf archive.tar.gz Pictures/


[bob@centos-host ~]$ tar tf archive.tar.gz 
Pictures/
Pictures/file2
Pictures/file4
Pictures/file5
Pictures/file1
Pictures/file3
```

________________________________________________________________________________________________

### decompress with `tar`

- `-a`    OR    `--autocompress`  (based on file extention)

  
```bash
[bob@centos-host ~]$ tar caf archive.tar.gz file1
```


________________________________________________________________________________________________


for decompression, you don't have to specify the format

```bash
[bob@centos-host ~]$ tar xf archive.tar.gz 
```

________________________________________________________________________________________________
