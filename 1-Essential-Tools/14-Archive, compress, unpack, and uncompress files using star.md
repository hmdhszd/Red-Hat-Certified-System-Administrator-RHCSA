

create archive

```bash
star -cv file=/home/hamid/archive.star file.txt
```

________________________________________________________________________________________________


list inside archive

```bash
star -tv file=/home/hamid/archive.star file.txt
```

________________________________________________________________________________________________


extract archive

```bash
star -xv file=/home/hamid/archive.star file.txt
```

```bash
star -xv file=/home/hamid/archive.star file.txt -c /tmp/
```

________________________________________________________________________________________________


compress and archive

```bash
star -cv file=/home/hamid/archive.star.gz file.txt
```

```bash
star -cv file=/home/hamid/archive.star.bz2 file.txt
```

________________________________________________________________________________________________


decompress

```bash
star -xv file=/home/hamid/archive.star.gz file.txt
```

________________________________________________________________________________________________



Use star to create an archive containing the files 1.txt through 10.txt. 


```bash
[bob@centos-host ~]$ star -cv file=/home/bob/archive1.star {1..10}.txt
```



