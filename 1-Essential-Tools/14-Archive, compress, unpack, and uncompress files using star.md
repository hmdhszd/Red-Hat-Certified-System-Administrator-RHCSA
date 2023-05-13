

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
