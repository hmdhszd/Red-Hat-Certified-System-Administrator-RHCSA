

### `SELinux` is `enabled` by `default` on CentOS stream


### it can protect against hijacked program.

________________________________________________________________________________________________

## `ls -Z`

### use -Z to see selinux permissions of a file (SELinux Context Label)

```bash
[bob@centos-host ~]$ ls -Z myfile

system_u:object_r:user_home_t:s0 myfile
```

`user`:`role`:`type`:`level` 


________________________________________________________________________________________________


#### `every user` that logs into a system, is `mapped` into a `selinux user`, as part of a selinux policy configuration

each user can only assume a `predefined set of roles`

________________________________________________________________________________________________


| SELinux User | Roles |
| :---: | :---: |
| developer_u | developer_r, docker_r |
| guest_u | guest_r |
| root | staff_r, sysadm_r, system_r, unconfined_r |


________________________________________________________________________________________________


type is like protective software jail


for `files` it's `type`, for `processes` it's `domain`


________________________________________________________________________________________________



1- only certain users can enter certain roles and certain types.

2- it lets authorized users and processes to do their jobs, by granting the permissions they need.

3- Authorized users and processes are allowed ONLY a limited set of actions.

4- Everything else is denied.


________________________________________________________________________________________________

## `ps -axZ`

### see processes with SELinux

```bash
[bob@centos-host ~]$ ps -axZ

LABEL                               PID TTY      STAT   TIME COMMAND
system_u:system_r:init_t:s0           1 ?        Ss     0:03 /usr/lib/syst
system_u:system_r:kernel_t:s0         2 ?        S      0:00 [kthreadd]
system_u:system_r:kernel_t:s0         3 ?        I<     0:00 [rcu_gp]
system_u:system_r:kernel_t:s0         4 ?        I<     0:00 [rcu_par_gp]
system_u:system_r:kernel_t:s0         6 ?        I<     0:00 [kworker/0:0H
system_u:system_r:kernel_t:s0         8 ?        R      0:00 [kworker/u2:0
system_u:system_r:kernel_t:s0         9 ?        I<     0:00 [mm_percpu_wq
system_u:system_r:kernel_t:s0        10 ?        S      0:00 [ksoftirqd/0]
system_u:system_r:kernel_t:s0        11 ?        R      0:00 [rcu_sched]
system_u:system_r:kernel_t:s0        12 ?        S      0:00 [migration/0]
system_u:system_r:kernel_t:s0        13 ?        S      0:00 [watchdog/0]
system_u:system_r:kernel_t:s0        14 ?        S      0:00 [cpuhp/0]
```

________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ ps -axZ | grep sshd

system_u:system_r:sshd_t:s0-s0:c0.c1023 8182 ?   Ss     0:00 /usr/sbin/sshd -D -u0 
```

________________________________________________________________________________________________


### only a file marked with a certain type can start a process that can shift into a certaint domain


```bash
[bob@centos-host ~]$ ls -Z /usr/sbin/sshd

system_u:object_r:sshd_exec_t:s0 /usr/sbin/sshd
```

________________________________________________________________________________________________


### anything labeled with `unconfined_t` is running `unrestricted`

```bash
unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023 24215 ? Ss   0:00 /usr/lib/systemd/
```

________________________________________________________________________________________________


## `security context` of the `current user` --> `id -Z`


```bash
[bob@centos-host ~]$ id -Z

system_u:system_r:initrc_t:s0
```

________________________________________________________________________________________________

## `semanage login -l` (`mapping` of the `current user`)

when we login, our user is automatically mapped to a SELinux user


```bash
[bob@centos-host ~]$ sudo semanage login -l

Login Name           SELinux User         MLS/MCS Range        Service

__default__          unconfined_u         s0-s0:c0.c1023       *
root                 unconfined_u         s0-s0:c0.c1023       *
```

________________________________________________________________________________________________



## `semanage user -l`  (`mapping` of the `all users`)

to see mapping of other users:

```bash
[bob@centos-host ~]$ sudo semanage user -l

                Labeling   MLS/       MLS/                          
SELinux User    Prefix     MCS Level  MCS Range                      SELinux Roles

guest_u         user       s0         s0                             guest_r
root            user       s0         s0-s0:c0.c1023                 staff_r sysadm_r system_r unconfined_r
staff_u         user       s0         s0-s0:c0.c1023                 staff_r sysadm_r unconfined_r
sysadm_u        user       s0         s0-s0:c0.c1023                 sysadm_r
system_u        user       s0         s0-s0:c0.c1023                 system_r unconfined_r
unconfined_u    user       s0         s0-s0:c0.c1023                 system_r unconfined_r
user_u          user       s0         s0                             user_r
xguest_u        user       s0         s0                             xguest_r
```

________________________________________________________________________________________________

## `getenforce`

to see if SELinux is enabled:

```bash
[bob@centos-host ~]$ getenforce

Enforcing
```

Enforcing     -->     denying unauthorized actions

Permissive    -->     allowing everything and log the restricted actions

Disabled      -->     not denying / not logging 


________________________________________________________________________________________________


## `sestatus`

```bash
[bob@centos-host ~]$ sestatus

SELinux status:                 enabled
SELinuxfs mount:                /sys/fs/selinux
SELinux root directory:         /etc/selinux
Loaded policy name:             targeted
Current mode:                   enforcing
Mode from config file:          enforcing
Policy MLS status:              enabled
Policy deny_unknown status:     allowed
Memory protection checking:     actual (secure)
Max kernel policy version:      32
```

________________________________________________________________________________________________



#        `chcon` - change file SELinux security context


Change the SELinux context of /var/index.html file to httpd_sys_content_t



```bash
[bob@centos-host ~]$ sudo chcon -t httpd_sys_content_t /var/index.html
```


```bash
[bob@centos-host ~]$ ls -Z /var/index.html

unconfined_u:object_r:httpd_sys_content_t:s0 /var/index.html
```


________________________________________________________________________________________________



# change SELinux Status

Temporarily change the SELinux status to Permissive on this system.




## `setenforce`


```bash
[bob@centos-host ~]$ sudo setenforce 0
```

________________________________________________________________________________________________
