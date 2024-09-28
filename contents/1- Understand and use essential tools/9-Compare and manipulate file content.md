


```bash
[bob@centos-host ~]$ cat myfile 
1
2
3
4
5
6
7
```

```bash
[bob@centos-host ~]$ tac myfile 
7
6
5
4
3
2
1
```

________________________________________________________________________________________________


```bash
[bob@centos-host ~]$ tail -n 20 /var/log/dnf.log
```

________________________________________________________________________________________________


```bash
[bob@centos-host ~]$ head -n 20 /var/log/dnf.log

```

________________________________________________________________________________________________


## `sed` (stream editor)

`replace` a `word` in a file:

`s`: `search` and replace

`g`: `global` (change all values) if we don't use g, it will change the first match only

```bash
[bob@centos-host ~]$ cat myfile 
        name    familyname      age
1       hamid   hszd            30
2       sdf     gfd             2354
3       hgds    dfgh            6543
4       hgdh    hhhhh           666
5       ssss    hhhjj           654
6       HAMID   hzzz            30
```


```bash
[bob@centos-host ~]$ sed 's/hamid/Hamid/g' myfile 
        name    familyname      age
1       Hamid   hszd            30
2       sdf     gfd             2354
3       hgds    dfgh            6543
4       hgdh    hhhhh           666
5       ssss    hhhjj           654
6       HAMID   hzzz            30

[bob@centos-host ~]$ cat myfile 
        name    familyname      age
1       hamid   hszd            30
2       sdf     gfd             2354
3       hgds    dfgh            6543
4       hgdh    hhhhh           666
5       ssss    hhhjj           654
6       HAMID   hzzz            30
```

________________________________________________________________________________________________

## `sed -i`

by default, it will not change the file, it only shows a preview

by using `-i` it will write the `changes` `in place` on the file:

```bash
[bob@centos-host ~]$ sed -i 's/hamid/Hamid/g' myfile

[bob@centos-host ~]$ cat myfile 
        name    familyname      age
1       Hamid   hszd            30
2       sdf     gfd             2354
3       hgds    dfgh            6543
4       hgdh    hhhhh           666
5       ssss    hhhjj           654
6       HAMID   hzzz            30
```

________________________________________________________________________________________________


## `cut` extract from a file

`d`: `delimiter`

`f`: `field`

```bash
[bob@centos-host ~]$ cut -d$'\t' -f 2 myfile 
name
Hamid
sdf
hgds
hgdh
ssss
HAMID
```

________________________________________________________________________________________________


### uniq is working only on sorted values

```bash
[bob@centos-host ~]$ cat namefile 
ali
hamid
ali
mamad
hamid
hali
reza
Hamid

[bob@centos-host ~]$ uniq namefile 
ali
hamid
ali
mamad
hamid
hali
reza
Hamid
```

________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ sort namefile 
Hamid
ali
ali
hali
hamid
hamid
mamad
reza

[bob@centos-host ~]$ sort namefile | uniq
Hamid
ali
hali
hamid
mamad
reza
```

________________________________________________________________________________________________





## `sort -duf`

Sort the contents of /home/bob/values.conf file alphabetically again, eliminate any common values and ignore case.


```bash
[bob@centos-host ~]$ sort -duf /home/bob/values.conf
```

```bash
       -d, --dictionary-order
              consider only blanks and alphanumeric characters

       -f, --ignore-case
              fold lower case to upper case characters
              
       -u, --unique
              with -c, check for strict ordering; without -c, output
              only the first of an equal run
```

________________________________________________________________________________________________





# Difference between 2 files:

- ## `diff`

- ## `diff -c`

- ## `diff -y`

- ## `sdiff`

________________________________________________________________________________________________


## `diff`

```bash
[bob@centos-host ~]$ cat namefile
ali
hamid
ali
mamad
hamid
hali
reza
Hamid

[bob@centos-host ~]$ cat namefile-old 
ali
HAAAMIIID
ali
mamad
hamid
hali
reza
Hamid

[bob@centos-host ~]$ diff namefile namefile-old 
2c2
< hamid
---
> HAAAMIIID
```

2c2 --> line 2 of file 1 and line 2 of file 2


________________________________________________________________________________________________


## `diff -c`


```bash
[bob@centos-host ~]$ diff -c namefile namefile-old 
*** namefile    2023-05-13 15:15:29.328306834 +0000
--- namefile-old        2023-05-13 15:17:53.681831214 +0000
***************
*** 1,5 ****
  ali
! hamid
  ali
  mamad
  hamid
--- 1,5 ----
  ali
! HAAAMIIID
  ali
  mamad
  hamid
```

________________________________________________________________________________________________


## `diff -y`

side by side diff

```bash
[bob@centos-host ~]$ diff -y namefile namefile-old 
ali                                                             ali
hamid                                                         | HAAAMIIID
ali                                                             ali
mamad                                                           mamad
hamid                                                           hamid
hali                                                            hali
reza                                                            reza
Hamid                                                           Hamid
```

## `sdiff`

```bash
[bob@centos-host ~]$ sdiff namefile namefile-old 
ali                                                             ali
hamid                                                         | HAAAMIIID
ali                                                             ali
mamad                                                           mamad
hamid                                                           hamid
hali                                                            hali
reza                                                            reza
Hamid                                                           Hamid
```

________________________________________________________________________________________________
