

find [path] [parameters]

without the "path" it will search in the current directory


________________________________________________________________________________________________



find by name and regex

```bash
[bob@centos-host ~]$ find /tmp/ -name '*.jpg'
/tmp/4.jpg
/tmp/2.jpg
/tmp/3.jpg
/tmp/1.jpg
```

________________________________________________________________________________________________


find files bigger that 10 Gig

```bash
[bob@centos-host ~]$ find /tmp -size +10G
```

   bigger than  +
   
   smaller than -

c   bytes

k   kilobytes

M   megabytes

G   gigabytes

________________________________________________________________________________________________


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


Search by the time modified:

search for files that modified in exact 5 mins ago (only on that on minute)

```bash
bob@centos-host ~]$ find -mmin 5
```

________________________________________________________________________________________________


search for the files which are modified in less than 5 minutes (from 5 mins ago until now)

```bash
bob@centos-host ~]$ find -mmin -5
```

________________________________________________________________________________________________


search by time of modified in the last days!

search for files that are modified in last 24 hours:

```bash
[bob@centos-host ~]$ find -mtime 0
```

________________________________________________________________________________________________



search for files that are modified from 48 hours ago to 24 hours ago:

```bash
[bob@centos-host ~]$ find -mtime 1
```

________________________________________________________________________________________________


### Modified time != Changed time

Modified time: when the contents of a file have been modified

Changed time:  when the Metadata of a file has been changed
```bash

```

________________________________________________________________________________________________



Search by the changed time:

search for files that is changed in the last 5 mins

```bash
bob@centos-host ~]$ find -cmin -5
```

________________________________________________________________________________________________


### multiple parameters:

by default it's AND operator:

```bash
[bob@centos-host ~]$ find -name "h*" -size +10M
```

but, we can use  -o   for OR operator:

```bash
[bob@centos-host ~]$ find -name "h*" -o -size +10M
```


________________________________________________________________________________________________


find all files that their name does NOT start with h :

```bash
[bob@centos-host ~]$ find -not -name "h*"
```

```bash
[bob@centos-host ~]$ find \! -name "h*"
```

________________________________________________________________________________________________


### search based of the permission of a file:

search for the exact permission:

```bash
[bob@centos-host ~]$ find -perm 644
```

```bash
[bob@centos-host ~]$ find -perm u=rw,g=rw,o=r
```

________________________________________________________________________________________________


search for the at least (minimum) permission: (this permission or higher)

```bash
[bob@centos-host ~]$ find -perm -644
```


```bash
[bob@centos-host ~]$ find -perm -u=rw,g=rw,o=r
```


________________________________________________________________________________________________


search for the any of these permissions: (it will be shown in the search if any of these permissions match)

```bash
[bob@centos-host ~]$ find -perm /644
```

________________________________________________________________________________________________


Find files/directories under /var/log/ directory that the group can write to, but others cannot read or write to it. 


```bash
sudo find /var/log/ -perm -g=w ! -perm /o=rw
```

the "-" means that "at least" : Permissions for the group have to be at least w

the "/" means "OR" : Permissions for others have not to be r or w. That means, if any of these two permissions, r or w match for others, the result has to be excluded.

________________________________________________________________________________________________


Find our secret file under /home/bob. You can either look for a file that is exactly 213 kilobytes or a file that has permission 402 in octal.

```bash
find /home/bob -size 213k -o -perm 402
```

________________________________________________________________________________________________


add SUID, GUID, StickyBit to a directory:

```bash
[bob@centos-host ~]$ chmod u+s,g+s,o+t /home/bob/datadir
```

________________________________________________________________________________________________


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


Create a directory named LFCS under bob's home directory and update its user owner permissions to only x (execute),
and group and others should not have any permissions.

```bash
[bob@centos-host ~]$ mkdir LFCS

[bob@centos-host ~]$ sudo chmod u=x,g=,o= LFCS/

[bob@centos-host ~]$ ls -ld LFCS/
d--x------ 2 100 bob 4096 May 13 13:29 LFCS/
```

________________________________________________________________________________________________
