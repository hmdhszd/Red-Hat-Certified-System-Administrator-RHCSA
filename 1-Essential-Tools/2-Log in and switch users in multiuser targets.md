

# user account
every user in linux has an associated account!

a user account maintains informations such as username and password

a user account containt an identifier called uid (unique for each user in the system)

user account is for individuals who needs access to the linux system

________________________________________________________________________________________________


the information of a user account is stored in /etc/passswd :

```bash
[bob@centos-host ~]$ cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
nobody:x:65534:65534:Kernel Overflow User:/:/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
systemd-coredump:x:999:997:systemd Core Dumper:/:/sbin/nologin
systemd-resolve:x:193:193:systemd Resolver:/:/sbin/nologin
tss:x:59:59:Account used for TPM access:/dev/null:/sbin/nologin
unbound:x:998:996:Unbound DNS resolver:/etc/unbound:/sbin/nologin
rpc:x:32:32:Rpcbind Daemon:/var/lib/rpcbind:/sbin/nologin
gluster:x:997:994:GlusterFS daemons:/run/gluster:/sbin/nologin
saslauth:x:996:76:Saslauthd user:/run/saslauthd:/sbin/nologin
polkitd:x:995:993:User for polkitd:/:/sbin/nologin
rpcuser:x:29:29:RPC Service User:/var/lib/nfs:/sbin/nologin
qemu:x:107:107:qemu user:/:/sbin/nologin
dnsmasq:x:992:992:Dnsmasq DHCP and DNS server:/var/lib/dnsmasq:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
pesign:x:991:990:Group for the pesign signing daemon:/run/pesign:/sbin/nologin
bob:x:1000:1000::/home/bob:/bin/bash
```

________________________________________________________________________________________________


# group

a linux group is a collection of users,
 
the information of the groups is stored at /etc/group :

```bash
[bob@centos-host ~]$ cat /etc/group
root:x:0:
bin:x:1:
daemon:x:2:
.
.
.
sshd:x:74:
stapusr:x:156:
stapsys:x:157:
stapdev:x:158:
pesign:x:990:
bob:x:1000:
```

________________________________________________________________________________________________

each group has a unique identifier called gid,

a user can be part of multiple groups

at the time of the user creation, if no groups is specified, the user will be assigned to the group with the same id and name as the user id (the primary gid of the user)

________________________________________________________________________________________________


the user account also stores the info of the home directory and the default shell of the user,

you can check it by running id command:

```bash
[bob@centos-host ~]$ whoami
bob
[bob@centos-host ~]$ id bob
uid=1000(bob) gid=1000(bob) groups=1000(bob)
```

________________________________________________________________________________________________


check the home directory:

```bash
[bob@centos-host ~]$ grep -i bob /etc/passwd
bob:x:1000:1000::/home/bob:/bin/bash
```

________________________________________________________________________________________________


# super user account

a super user account is the root (uid = 0)

it has unrestricted access to the system

________________________________________________________________________________________________


# system accounts

system accounts are usually created during the system instalation,

they are used by softwares and services that will not run as user user (ssh / mail)

uid < 100  OR   between 500-1000

they usually don't have home directory


________________________________________________________________________________________________


# service accounts

service accounts are similar to system accounts, and are created when services are installed in linux (nginx)

________________________________________________________________________________________________


list of users who are logged in:

```bash
[bob@centos-host ~]$ who
pts/1      Nov  9 00:20   long_username_greater_than_eight_characters  (localhost)
```

________________________________________________________________________________________________


display the logs of all logged in users: (with the date and time when the system is rebooted)

```bash
[bob@centos-host ~]$ last
reboot   system boot  5.4.0-1104-gcp   Fri May 12 18:34   still running

wtmp begins Fri May 12 18:34:11 2023
```

________________________________________________________________________________________________


switch to the root user:

```bash
[bob@centos-host ~]$ su -
Password:
```

________________________________________________________________________________________________


run a command with the root previlages:

```bash
[bob@centos-host ~]$ su -c "whoami"
Password:

root
```

OR


```bash
[bob@centos-host ~]$ sudo whoami
root
```

________________________________________________________________________________________________


the default configuration of the sudo is in /etc/sudoers

only users defined in this file can use the sudo command

this file can be updated using "visudo" command.

```bash
[bob@centos-host ~]$ sudo cat /etc/sudoers | grep -v "#"


Defaults   !visiblepw

Defaults    always_set_home
Defaults    match_group_by_gid

Defaults    always_query_group_plugin

Defaults    env_reset
Defaults    env_keep =  "COLORS DISPLAY HOSTNAME HISTSIZE KDEDIR LS_COLORS"
Defaults    env_keep += "MAIL PS1 PS2 QTDIR USERNAME LANG LC_ADDRESS LC_CTYPE"
Defaults    env_keep += "LC_COLLATE LC_IDENTIFICATION LC_MEASUREMENT LC_MESSAGES"
Defaults    env_keep += "LC_MONETARY LC_NAME LC_NUMERIC LC_PAPER LC_TELEPHONE"
Defaults    env_keep += "LC_TIME LC_ALL LANGUAGE LINGUAS _XKB_CHARSET XAUTHORITY"


Defaults    secure_path = /sbin:/bin:/usr/sbin:/usr/bin

root    ALL=(ALL)       ALL


%wheel  ALL=(ALL)       ALL




bob    ALL=(ALL)   NOPASSWD:ALL
```

________________________________________________________________________________________________


we can ban the root user from login, for security reasons:

```bash
[bob@centos-host ~]$ grep -i root /etc/passwd
root:x:0:0:root:/root:/sbin/nologin
```

________________________________________________________________________________________________


in the sudoers file:

bob    ALL=(ALL)   NOPASSWD:ALL


the first field is the user or group, (group begin with percentage %    ex:   %admin)

the second field is the host, (ex: localhost, ALL(default) )

the third field is the user or group that the user in the first field can run the command as (ex: ALL(default) )

the forth field is the command that can be run ( /bin/ls , ALL(default) )

________________________________________________________________________________________________


for example, user hamid has only permission to user shutdown command:



```bash
hamid localhost=/usr/bin/shutdown -r now
```
