
## `#!/bin/bash`


________________________________________________________________________________________________




## if statement

`if` TEST-COMMAND1

`then`

  STATEMENTS1

`elif` TEST-COMMAND2

`then`

  STATEMENTS2

`else`

  STATEMENTS3

`fi`



________________________________________________________________________________________________



## `test -f myfile`

to check if a `file` `exists`


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


- [ "abc" = "abc" ]

- [ "abc" != "abc" ]

- [ 5 -eq 5 ]

- [ 5 -ne 5 ]

- [ 5 -gt 4 ]

- [ 5 -lt 6 ]




________________________________________________________________________________________________




- [[ "abcd" = *bc* ]]

- [[ "abc" = ab[cd] ]]

- [[ "abc" = ab[cd] ]]

- [[ "abc" < "bcd" ]]

- [ cond1 ] && [ cond2 ]

- [[ cond1 && cond2 ]]

- [ cond1 ] || [ cond2 ]

- [[ cond1 || cond2 ]]



________________________________________________________________________________________________


- `[ -e FILE ]`         if the `file` exists


- `[ -d DIRECROTY ]`         if the `directory` exists


- `[ -s FILE ]`         if the `file` exists and it's `size` is `bigger` that `0`


- `[ -x FILE ]`         if the `file` is `executable`


- `[ -w FILE ]`         if the `file` is `writable`



________________________________________________________________________________________________


### exist codes

0     --->    OK

greater than 0   --->    ERROR


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
then
  .......
else
  exit 1
fi
```


________________________________________________________________________________________________

## `read -p`

`input` value

```bash
read -p "Enter your name: " name
echo $name
```

________________________________________________________________________________________________

## UpperCase to LowerCase

```bash
echo "THIS IS MY DATA" | tr [:upper:] [:lower:]
```

________________________________________________________________________________________________



Make the shell variable named VARIABLE visible to subshells:

```bash
export VARIABLE
```

________________________________________________________________________________________________



Add a new environment variable “VAR” with the value “RHCSA” which will be available for local sessions for all users:


Edit the global bash configuration file:

```bash
vim /etc/bashrc

export VAR="RHCSA"
```

```bash
source /etc/bashrc

echo $VAR
```



________________________________________________________________________________________________




Write a script that creates a user examuser with a home directory /home/examuser and adds the user to a group called examgroup. The password for the user should be set to P@ssw0rd123.

Answer:

```bash
#!/bin/bash
groupadd examgroup
useradd -m -d /home/examuser -G examgroup examuser
echo "P@ssw0rd123" | passwd --stdin examuser
echo "User and group created."
```

________________________________________________________________________________________________


