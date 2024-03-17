

## `useradd`

```bash
[bob@centos-host ~]$ sudo useradd hamid
```

user and group hamid will be created, the primary group of this user will be hamid

the home directory /home/hamid will be created

 default shell /bin/bash
 
 

________________________________________________________________________________________________



## `/etc/passwd`

the info of users will be saved at /etc/passwd

```bash
[bob@centos-host ~]$ cat /etc/passwd | grep hamid
hamid:x:1002:1002::/home/hamid:/bin/bash
```

________________________________________________________________________________________________



## `/etc/skel`

and these files from /etc/skel will be copied to the `home directory` of new users (/home/hamid)

```bash
[bob@centos-host ~]$ ls -a /etc/skel
.  ..  .bash_logout  .bash_profile  .bashrc
```

________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ sudo ls -a /home/hamid
.  ..  .bash_logout  .bash_profile  .bashrc
```

________________________________________________________________________________________________



## `useradd --defaults`

### show default options for the new users

```bash
[bob@centos-host ~]$ useradd --defaults

GROUP=100
HOME=/home
INACTIVE=-1
EXPIRE=
SHELL=/bin/bash
SKEL=/etc/skel
CREATE_MAIL_SPOOL=yes
```

________________________________________________________________________________________________


## `/etc/login.defs`


other `default configurations` related to `new users`


```bash
[bob@centos-host ~]$ cat /etc/login.defs 
#
# Please note that the parameters in this configuration file control the
# behavior of the tools from the shadow-utils component. None of these
# tools uses the PAM mechanism, and the utilities that use PAM (such as the
# passwd command) should therefore be configured elsewhere. Refer to
# /etc/pam.d/system-auth for more information.
#
.
.
.
```

________________________________________________________________________________________________




## `passwd`

by default, the accounts does not have any passwords, to set password:

```bash
[bob@centos-host ~]$ sudo passwd hamid

Changing password for user hamid.
New password: 
BAD PASSWORD: The password fails the dictionary check - it is too simplistic/systematic
Retype new password: 
passwd: all authentication tokens updated successfully.
```

________________________________________________________________________________________________




## `userdel`

be default, this command does not remove home directory

```bash
[bob@centos-host ~]$ sudo userdel hamid
```

________________________________________________________________________________________________



## `userdel --remove`

to `remove` `home directory` as well (by default, it does not remove the home directory)

```bash
[bob@centos-host ~]$ sudo userdel --remove hamid
```


## `userdel -r`

```bash
[bob@centos-host ~]$ sudo userdel -r hamid
```

________________________________________________________________________________________________




## `useradd --shell --home-dir`

define other shell and home dir at user creation

```bash
[bob@centos-host ~]$ sudo useradd --shell /bin/OTHERSHELL --home-dir /home/OTHERDIR hamid
```

________________________________________________________________________________________________




## `useradd --uid`

define other UID at user creation

```bash
[bob@centos-host ~]$ sudo useradd --uid 1100 hamid
```

________________________________________________________________________________________________



## `id`

info of the `current user`

```bash
[bob@centos-host ~]$ id

uid=1000(bob) gid=1000(bob) groups=1000(bob)
```

________________________________________________________________________________________________





## `whoami`

see the current user

```bash
[bob@centos-host ~]$ whoami

bob
```

________________________________________________________________________________________________


# Create a System Account



## `useradd --system`

system accounts will hame `NO` `home` `directory`

it's usually used by `daemons` 
 

```bash
[bob@centos-host ~]$ sudo useradd --system my-system-account
```

________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ cat /etc/passwd | grep my

my-system-account:x:990:989::/home/my-system-account:/bin/bash
```

________________________________________________________________________________________________


### MODIFY a user by usermod

```bash
[bob@centos-host ~]$ usermod --help
Usage: usermod [options] LOGIN

Options:
  -c, --comment COMMENT         new value of the GECOS field
  -d, --home HOME_DIR           new home directory for the user account
  -e, --expiredate EXPIRE_DATE  set account expiration date to EXPIRE_DATE
  -f, --inactive INACTIVE       set password inactive after expiration
                                to INACTIVE
  -g, --gid GROUP               force use GROUP as new primary group
  -G, --groups GROUPS           new list of supplementary GROUPS
  -a, --append                  append the user to the supplemental GROUPS
                                mentioned by the -G option without removing
                                the user from other groups
  -h, --help                    display this help message and exit
  -l, --login NEW_LOGIN         new value of the login name
  -L, --lock                    lock the user account
  -m, --move-home               move contents of the home directory to the
                                new location (use only with -d)
  -o, --non-unique              allow using duplicate (non-unique) UID
  -p, --password PASSWORD       use encrypted password for the new password
  -R, --root CHROOT_DIR         directory to chroot into
  -P, --prefix PREFIX_DIR       prefix directory where are located the /etc/* files
  -s, --shell SHELL             new login shell for the user account
  -u, --uid UID                 new UID for the user account
  -U, --unlock                  unlock the user account
  -v, --add-subuids FIRST-LAST  add range of subordinate uids
  -V, --del-subuids FIRST-LAST  remove range of subordinate uids
  -w, --add-subgids FIRST-LAST  add range of subordinate gids
  -W, --del-subgids FIRST-LAST  remove range of subordinate gids
  -Z, --selinux-user SEUSER     new SELinux user mapping for the user account
```

________________________________________________________________________________________________



## `usermod  --home  --move-home`

change the home dir

```bash
[bob@centos-host ~]$ sudo usermod  --home /home/OTHERDIR --move-home hamid
```

OR


## `usermod  --d  -m`

```bash
[bob@centos-host ~]$ sudo usermod  --d /home/OTHERDIR -m hamid
```


________________________________________________________________________________________________




## `usermod --login`

change the `username` from hamid to mrx

```bash
[bob@centos-host ~]$ sudo usermod --login mrx hamid
```

OR



## `usermod -l`


```bash
[bob@centos-host ~]$ sudo usermod -l mrx hamid

```

________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ cat /etc/passwd | grep mrx

mrx:x:1002:1002::/home/hamid:/bin/bash
```

________________________________________________________________________________________________



## `usermod --shell`

change the login shell

```bash
[bob@centos-host ~]$ sudo usermod --shell /bin/OTHERSHELL hamid
```

OR




## `usermod -s`

```bash
[bob@centos-host ~]$ sudo usermod -s /bin/OTHERSHELL hamid
```

________________________________________________________________________________________________



## `usermod --lock`

### Lock / Disable account

```bash
[bob@centos-host ~]$ sudo usermod --lock hamid
```

________________________________________________________________________________________________




## `usermod --unlock`

### Un Lock / Enable account

```bash
[bob@centos-host ~]$ sudo usermod --unlock hamid
```

________________________________________________________________________________________________



## `usermod --expiredate`


### Set `USER` `expiration` date

```bash
[bob@centos-host ~]$ sudo usermod --expiredate 2023-11-12 hamid
```

________________________________________________________________________________________________


`remove` expiration date


```bash
[bob@centos-host ~]$ sudo usermod --expiredate "" hamid
```

________________________________________________________________________________________________




## `chage --lastday`

### set `Password` `expiration`


expire right now

```bash
[bob@centos-host ~]$ sudo chage --lastday 0 hamid
```

________________________________________________________________________________________________



unexpire password (`-1`)

```bash
[bob@centos-host ~]$ sudo chage --lastday -1 hamid
```

________________________________________________________________________________________________


## `chage --maxdays`


`Password` `expiration` in `maximum` 30 days

```bash
[bob@centos-host ~]$ sudo chage --maxdays 30 hamid
```

________________________________________________________________________________________________


## `chage --list`

#### see the `expiration` `policies` of a user

```bash
[bob@centos-host ~]$ sudo chage --list mrx

Last password change                                    : password must be changed
Password expires                                        : password must be changed
Password inactive                                       : password must be changed
Account expires                                         : never
Minimum number of days between password change          : 0
Maximum number of days between password change          : 99999
Number of days of warning before password expires       : 7
```

________________________________________________________________________________________________


### Ensure that a file named "HelloNewUser" is automatically added to the home folders of all new users.



Explanation
1. Create a new file called "Congrats" in the `/etc/skel` directory:

```bash
touch /etc/skel/HelloNewUser
```


2. Verify file permissions (optional):

```bash
chmod 644 /etc/skel/Congrats
```






________________________________________________________________________________________________


## `/etc/login.defs`

### Enforce `password expiration` after 90 days and a `minimum length` of 8 characters for `all` `user` passwords.



1. In configuration file for login settings (`/etc/login.defs`), Change the value of `PASS_MAX_DAYS` to 90:



4. In the `/etc/security/pwquality.conf` file, Set the value of `minlen` to 8 :


Note:


- These changes will only affect newly created or changed passwords. To force immediate expiration of existing passwords, use the chage command: `$ sudo chage -M 0 <username>`




________________________________________________________________________________________________


## `pwquality.conf`

### Examples of configuration parameters that can be modified in the `pwquality.conf` file include:

`minlen`: Sets the minimum `password` `length`.

`ucredit`: Specifies the minimum number of `uppercase` `letters` required in a password.

`dcredit`: Specifies the minimum number of `digits` required in a password.

`ocredit`: Specifies the minimum number of `special` characters required in a password.

`lcredit`: Specifies the minimum number of `lowercase` letters required in a password.

`retry`: Specifies the number of times a user can `attempt` to enter their password before being locked out.






________________________________________________________________________________________________



### Create a user john with UID 1250 and expiry date 2023-12-21.

```bash
useradd -u 1250 -e 2023-12-21 john
```

Verify:

```bash
id john

chage -l john
```


________________________________________________________________________________________________


### Create a user hamid whose UID is 5000, and he doesn't have access to any interactive shell on the system.

```bash
useradd -u 5000 -s /sbin/nologin hamid
```



________________________________________________________________________________________________



