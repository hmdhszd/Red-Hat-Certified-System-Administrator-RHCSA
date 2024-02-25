

## `find`

find [path] [parameters]

without the "path" it will search in the current directory


________________________________________________________________________________________________

## `find -name`


find by name and regex

```bash
[bob@centos-host ~]$ find /tmp/ -name '*.jpg'
/tmp/4.jpg
/tmp/2.jpg
/tmp/3.jpg
/tmp/1.jpg
```

________________________________________________________________________________________________


## `find -size`

find files bigger that 10 Gig

```bash
[bob@centos-host ~]$ find /tmp -size +10G
```

   `bigger` than  `+`
   
   `smaller` than `-`

- `c`   `bytes`


- `k`   `kilobytes`


- `M`   `megabytes`


- `G`   `gigabytes`

________________________________________________________________________________________________


## `find -iname`



find with name (key insensitive) -i

```bash
[bob@centos-host ~]$ touch Hamid
[bob@centos-host ~]$ touch hamid
[bob@centos-host ~]$ touch HAMID

[bob@centos-host ~]$ find -iname hamid
./HAMID
./hamid
./Hamid
```

________________________________________________________________________________________________


it can be any charachter or nothing!

```bash
[bob@centos-host ~]$ find -name 'h*'
./hh
./hamid
./hammmmid
```

________________________________________________________________________________________________


## `find -mmin`



Search by the time `modified`:

search for files that modified in `exact` 5 `minutes` ago (only on that on minute)

```bash
bob@centos-host ~]$ find -mmin 5
```

________________________________________________________________________________________________


search for the files which are modified in `less than` 5 `minutes` (from 5 mins ago until now)

```bash
bob@centos-host ~]$ find -mmin -5
```

________________________________________________________________________________________________



## `find -mtime`




search by time of modified in the `last` `day`!

search for files that are modified in `last` `24 hours`:

```bash
[bob@centos-host ~]$ find -mtime 0
```

________________________________________________________________________________________________



search for files that are modified from 48 hours ago to 24 hours ago:

```bash
[bob@centos-host ~]$ find -mtime 1
```

________________________________________________________________________________________________


### Modified != Changed 

`Modified` : when the `Contents` of a file have been modified

`Changed` :  when the `Metadata` of a file has been changed



----------


min --> Minute

- mmin --> Modified Minute

- cmin --> Changed Minute



time --> day

- mtime --> Modified Day

- ctime --> Changed Day


________________________________________________________________________________________________



## `find -cmin`



Search by the changed time:

search for files that is changed in `less than` 5 `minutes` (from 5 mins ago until now)

```bash
bob@centos-host ~]$ find -cmin -5
```

________________________________________________________________________________________________


### multiple parameters:

by default it's `AND` operator:

```bash
[bob@centos-host ~]$ find -name "h*" -size +10M
```

but, we can use  `-o`  for `OR` operator:

```bash
[bob@centos-host ~]$ find -name "h*" -o -size +10M
```


________________________________________________________________________________________________


find all files that their name does NOT start with h :

`NOT`:

- `-not`

- `\!` and `!`


```bash
[bob@centos-host ~]$ find -not -name "h*"
```

```bash
[bob@centos-host ~]$ find \! -name "h*"
```

________________________________________________________________________________________________

## `find -perm`

### search based of the permission of a file:

search for the `exact` `permission`:

```bash
[bob@centos-host ~]$ find -perm 644
```

```bash
[bob@centos-host ~]$ find -perm u=rw,g=rw,o=r
```

________________________________________________________________________________________________


search for the at least (`minimum`) `permission`: `-` (this permission or higher)

```bash
[bob@centos-host ~]$ find -perm -644
```


```bash
[bob@centos-host ~]$ find -perm -u=rw,g=rw,o=r
```


________________________________________________________________________________________________


search for the `ANY` of these `permissions`: `/` (it will be shown in the search if any of these permissions match)

```bash
[bob@centos-host ~]$ find -perm /644
```

________________________________________________________________________________________________


Find files/directories under /var/log/ directory that the group can write to, but others cannot read or write to it. 


```bash
sudo find /var/log/ -perm -g=w ! -perm /o=rw
```

the "-" means that "at least" : Permissions for the group have to be at least w

the "/" means "ANY" and "!" means "NOT": Permissions for others have not to be neither r nor w. That means, if any of these two permissions, r or w match for others, the result has to be excluded.

________________________________________________________________________________________________


Find our secret file under /home/bob. You can either look for a file that is exactly 213 kilobytes or a file that has permission 402 in octal.

```bash
find /home/bob -size 213k -o -perm 402
```

________________________________________________________________________________________________

## `find -type`

- `f` file

- `d` directory

Find all the files whose permissions are 0777 in /var directory.

```bash
[bob@centos-host ~]$ sudo find /var -type f -perm 0777 
```

________________________________________________________________________________________________


Find all files between 5mb and 10mb in the /usr directory

```bash
[bob@centos-host ~]$ sudo find /usr -type f -size +5M -size -10M
```

________________________________________________________________________________________________


Find cats.txt file under bob's home directory and copy it into /opt directory.




```bash
[bob@centos-host ~]$ sudo find /home/bob -type f -name cats.txt -exec cp {} /opt/ \;
```

________________________________________________________________________________________________



find the files owned by the user root in the “/usr/bin” and copy the files into the “/find/rootfiles/” directory.



```bash
[bob@centos-host ~]$ sudo find /usr/bin/ -type f -user root -exec cp {} /find/rootfiles/ \;
```




________________________________________________________________________________________________



find and delete all empty files in /tmp.



```bash
[bob@centos-host ~]$ sudo find /tmp -type f -empty -delete
```

________________________________________________________________________________________________



find the files owned by root and with the SUID bit set in /usr.



```bash
[bob@centos-host ~]$ sudo find /usr -uid 0 -perm -4000
```


________________________________________________________________________________________________

