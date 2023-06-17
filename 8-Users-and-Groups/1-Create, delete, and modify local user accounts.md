

### add new user

```bash
[bob@centos-host ~]$ sudo useradd hamid
```

user and group hamid will be created, the primary group of this user will be hamid

the home directory /home/hamid will be created

 default shell /bin/bash
 
 

________________________________________________________________________________________________


the info of users will be saved at /etc/passwd

```bash
[bob@centos-host ~]$ cat /etc/passwd | grep hamid
hamid:x:1002:1002::/home/hamid:/bin/bash
```

________________________________________________________________________________________________


and these files from /etc/skel will be copied to the /home/hamid

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


### default options for the new users

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


other default configurations related to new users


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


### by default, the accounts does not have any passwords, to set password:

```bash
[bob@centos-host ~]$ sudo passwd hamid

Changing password for user hamid.
New password: 
BAD PASSWORD: The password fails the dictionary check - it is too simplistic/systematic
Retype new password: 
passwd: all authentication tokens updated successfully.
```

________________________________________________________________________________________________


### delete a user

be default, this command does not remove home directory

```bash
[bob@centos-host ~]$ sudo userdel hamid
```

________________________________________________________________________________________________


to remove home directory as well

```bash
[bob@centos-host ~]$ sudo userdel --remove hamid
```

```bash
[bob@centos-host ~]$ sudo userdel -r hamid
```

________________________________________________________________________________________________


### define other shell and home dir at user creation

```bash
[bob@centos-host ~]$ sudo useradd --shell /bin/OTHERSHELL --home-dir /home/OTHERDIR hamid
```

________________________________________________________________________________________________


### define other UID at user creation

```bash
[bob@centos-host ~]$ sudo useradd --uid 1100 hamid
```

________________________________________________________________________________________________


### info of the current user

```bash
[bob@centos-host ~]$ id

uid=1000(bob) gid=1000(bob) groups=1000(bob)
```

________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ whoami

bob
```

________________________________________________________________________________________________


### Create a System Account


system accounts will not have any home dir

usually daemons will use system accounts
 

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


change the home dir

```bash
[bob@centos-host ~]$ sudo usermod  --home /home/OTHERDIR --move-home hamid
```

OR

```bash
[bob@centos-host ~]$ sudo usermod  --d /home/OTHERDIR -m hamid
```


________________________________________________________________________________________________


change the username from hamid to mrx

```bash
[bob@centos-host ~]$ sudo usermod --login mrx hamid
```

OR



```bash
[bob@centos-host ~]$ sudo usermod -l mrx hamid

```

________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ cat /etc/passwd | grep mrx

mrx:x:1002:1002::/home/hamid:/bin/bash
```

________________________________________________________________________________________________


change the login shell

```bash
[bob@centos-host ~]$ sudo usermod --shell /bin/OTHERSHELL hamid
```

OR



```bash
[bob@centos-host ~]$ sudo usermod -s /bin/OTHERSHELL hamid
```

________________________________________________________________________________________________


### Lock / Disable account

```bash
[bob@centos-host ~]$ sudo usermod --lock hamid
```

________________________________________________________________________________________________



### Un Lock / Enable account

```bash
[bob@centos-host ~]$ sudo usermod --unlock hamid
```

________________________________________________________________________________________________



### Set expire date

```bash
[bob@centos-host ~]$ sudo usermod --expiredate 2023-11-12 hamid
```

________________________________________________________________________________________________


remove expiration date


```bash
[bob@centos-host ~]$ sudo usermod --expiredate "" hamid
```

________________________________________________________________________________________________



### set password expiration


expire right now

```bash
[bob@centos-host ~]$ sudo chage --lastday 0 hamid
```

________________________________________________________________________________________________



unexpire password

```bash
[bob@centos-host ~]$ sudo chage --lastday -1 hamid
```

________________________________________________________________________________________________



expire in maximum 30 days

```bash
[bob@centos-host ~]$ sudo chage --maxdays 30 hamid
```

________________________________________________________________________________________________



#### see the password expiration policies of a user

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
