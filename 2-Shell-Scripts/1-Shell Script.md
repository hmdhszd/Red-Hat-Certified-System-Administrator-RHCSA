

to check if a file exists

test -f myfile

```bash
[bob@centos-host ~]$ nano my-script.sh

#!/bin/sh
if test -f /tmp/archive.tar.gz;
then
        mv /tmp/archive.tar.gz /tmp/archive.tar.gz-OLD
        tar acf /tmp/archive.tar.gz /etc/dnf
else
        tar acf /tmp/archive.tar.gz /etc/dnf
fi

```


```bash
[bob@centos-host ~]$ chmod +x my-script.sh 
[bob@centos-host ~]$ ./my-script.sh
```

________________________________________________________________________________________________


### conditional statement


[ "abc" = "abc" ]

[ "abc" != "abc" ]

[ 5 -eq 5 ]

[ 5 -ne 5 ]

[ 5 -gt 4 ]

[ 5 -lt 6 ]




________________________________________________________________________________________________




[[ "abcd" = *bc* ]]

[[ "abc" = ab[cd] ]]

[[ "abc" = ab[cd] ]]

[[ "abc" < "bcd" ]]

[ cond1 ] && [ cond2 ]

[[ cond1 && cond2 ]]

[ cond1 ] || [ cond2 ]

[[ cond1 || cond2 ]]



________________________________________________________________________________________________


[ -e FILE ]         if the file exists


[ -d FILE ]         if the directory exists


[ -s FILE ]         if the file exists and it's size is bigger that 0


[ -x FILE ]         if the file is executable


[ -w FILE ]         if the is writable



________________________________________________________________________________________________


### exist codes

0     --->    OK

1/2   --->    ERROR


________________________________________________________________________________________________



```bash
[bob@centos-host ~]$ ls
dir1  my-script.sh
[bob@centos-host ~]$ echo $?
0
```

```bash
[bob@centos-host ~]$ lssss
-bash: lssss: command not found
[bob@centos-host ~]$ echo $?
127
```

________________________________________________________________________________________________


always exit with an exit code

```bash
if  condition;
  .......
else
  exit 1
fi
```


________________________________________________________________________________________________


#### read / input

```bash
read -p "Enter your name: " name
echo $name
```

________________________________________________________________________________________________
