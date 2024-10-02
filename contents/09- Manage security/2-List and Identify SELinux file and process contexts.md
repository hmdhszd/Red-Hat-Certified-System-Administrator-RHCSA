**SELinux (Security-Enhanced Linux)** is a mandatory access control (MAC) security system implemented in the Linux kernel. It enforces security policies that dictate which users and processes can access various system resources. This is a critical topic for the **Red Hat Certified System Administrator (RHCSA) Exam**, and understanding how to work with SELinux is essential.

### Why SELinux?

Unlike traditional discretionary access control (DAC) systems like file permissions (read, write, execute), SELinux adds an additional layer of security that restricts processes from performing unauthorized actions. Even if a process has the necessary DAC permissions, SELinux policies can block it if the action violates system security policies.

### Key Concepts of SELinux:
1. **SELinux Modes**:
   - **Enforcing**: SELinux policies are applied, and violations are blocked and logged.
   - **Permissive**: Violations are not blocked, but they are logged for debugging purposes.
   - **Disabled**: SELinux is turned off.

2. **SELinux Contexts**: 
   Every file, directory, and process in a system has an SELinux context, which consists of:
   - **User**: Represents the SELinux user (not to be confused with a Linux user).
   - **Role**: Specifies the role of the object (often fixed, rarely modified by users).
   - **Type**: A crucial part, also called the *type enforcement* mechanism. Objects (files) have a type and processes (domains) have types. The SELinux policies control which processes (domains) can access which objects (types).
   - **MLS (Multi-Level Security)**: Used in specialized security setups (not a focus for RHCSA).

   For example, the context of a file might look like this:
   ```
   system_u:object_r:httpd_sys_content_t:s0
   ```
   see all the types of httpd:
      ```bash
      [root@Server2 ~]# semanage fcontext -l | awk '{print $4}' | grep httpd | sort | uniq
      system_u:object_r:httpd_cache_t:s0
      system_u:object_r:httpd_config_t:s0
      system_u:object_r:httpd_exec_t:s0
      system_u:object_r:httpd_helper_exec_t:s0
      system_u:object_r:httpd_initrc_exec_t:s0
      system_u:object_r:httpd_keytab_t:s0
      system_u:object_r:httpd_log_t:s0
      system_u:object_r:httpd_modules_t:s0
      system_u:object_r:httpd_passwd_exec_t:s0
      system_u:object_r:httpd_rotatelogs_exec_t:s0
      system_u:object_r:httpd_squirrelmail_t:s0
      system_u:object_r:httpd_suexec_exec_t:s0
      system_u:object_r:httpd_sys_content_t:s0
      system_u:object_r:httpd_sys_rw_content_t:s0
      system_u:object_r:httpd_sys_script_exec_t:s0
      system_u:object_r:httpd_tmp_t:s0
      system_u:object_r:httpd_unit_file_t:s0
      system_u:object_r:httpd_var_lib_t:s0
      system_u:object_r:httpd_var_run_t:s0
      unconfined_u:object_r:httpd_user_content_t:s0
      unconfined_u:object_r:httpd_user_htaccess_t:s0
      unconfined_u:object_r:httpd_user_ra_content_t:s0
      unconfined_u:object_r:httpd_user_script_exec_t:s0
      ```

4. **SELinux Booleans**:
   Booleans are used to toggle specific features of SELinux on and off, allowing more granular control without changing the SELinux policy.

5. **SELinux Policies**:
   These define what types of interactions between processes and objects are allowed or denied. Policies are preconfigured and can be enforced or modified to some extent using Booleans.

---

### SELinux Commands for RHCSA

Below are the important SELinux-related commands and tools that you’ll need for the RHCSA exam:

#### 1. **Check SELinux Status**

To verify SELinux status and mode, use the `sestatus` command:
```bash
sestatus
```

Example output:
```
SELinux status:                 enabled
SELinuxfs mount:                /sys/fs/selinux
SELinux root directory:         /etc/selinux
Loaded policy name:             targeted
Current mode:                   enforcing
Mode from config file:          enforcing
Policy MLS status:              enabled
Policy deny_unknown status:     allowed
Max kernel policy version:      31
```
This will tell you if SELinux is enabled and which mode it's in.

#### 2. **Switch SELinux Mode Temporarily**
You can temporarily change the SELinux mode (until next reboot) using the `setenforce` command:
- **Set SELinux to permissive mode** (temporarily disables enforcement but logs violations):
  ```bash
  sudo setenforce 0
  ```

- **Set SELinux to enforcing mode**:
  ```bash
  sudo setenforce 1
  ```

To verify the mode after changing it:
```bash
sestatus
```

#### 3. **Set SELinux Mode Permanently**
To permanently change the SELinux mode (survives reboot), edit the SELinux configuration file `/etc/selinux/config` and change the `SELINUX` directive:
```bash
vi /etc/selinux/config
```

Example file content:
```
# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
SELINUX=enforcing
SELINUXTYPE=targeted
```
You can change `SELINUX` to `permissive` or `disabled` if necessary.

#### 4. **View File and Process Contexts**

To view the SELinux context of files, use the `-Z` option with `ls`:
```bash
ls -Z /var/www/html
```

Sample output:
```
-rw-r--r--. root root unconfined_u:object_r:httpd_sys_content_t:s0 index.html
```
The important part here is the `httpd_sys_content_t` type. This indicates that the file is accessible by the `httpd` service.

To view the SELinux context of a running process, use `ps -Z`:
```bash
ps -eZ | grep httpd
```

Sample output:
```
system_u:system_r:httpd_t:s0 1731 ? 00:00:00 httpd
```
The process type `httpd_t` shows the domain in which this process is running.

#### 5. **Fixing SELinux Contexts (Relabeling Files)**

Sometimes files can have incorrect SELinux contexts, and this can block services from accessing files even if they have the correct file permissions. You can reset the context of a file using `restorecon`:
```bash
sudo restorecon -v /var/www/html/index.html
```

This command will restore the default SELinux context for the file.

To relabel an entire directory:
```bash
sudo restorecon -R -v /var/www/html
```



### **2. allow an Apache web server to connect to a backend database**
You are instructed to allow an Apache web server to connect to a backend database server over the network.
   - **Solution**: Enable the `httpd_can_network_connect` Boolean:
     ```bash
     sudo setsebool -P httpd_can_network_connect on
     ```
---


### **4. Changing SELinux Contexts Using `chcon`**

#### Sample Question:
You are hosting a custom script `/srv/myapp/run.sh` that should be executable by Apache (`httpd`). Currently, the script has a context that prevents it from being executed by Apache. Set the correct SELinux context.

#### Solution:
1. Check the current context:
   ```bash
   ls -Z /srv/myapp/run.sh
   ```
   You might see:
   ```
   -rwxr-xr-x. root root unconfined_u:object_r:default_t:s0 run.sh
   ```

2. Use `chcon` to set the correct context for Apache (which should be `httpd_sys_script_exec_t`):
   ```bash
   sudo chcon -t httpd_sys_script_exec_t /srv/myapp/run.sh
   ```

3. Verify the context change:
   ```bash
   ls -Z /srv/myapp/run.sh
   ```
   Correct output:
   ```
   -rwxr-xr-x. root root unconfined_u:object_r:httpd_sys_script_exec_t:s0 run.sh
   ```

#### Notes:
- **`chcon`** changes SELinux context temporarily, and it will be lost after a relabel or reboot.
- For a permanent solution, use the `semanage` tool as shown in the next section.

---

### **7. Troubleshooting SELinux Issues (Audit Logs)**

#### Sample Question:
An application is failing to work, and you suspect SELinux is blocking the operation. Use the `audit.log` to identify and fix the issue.

#### Solution:
1. Check the audit logs for recent SELinux denials:
   ```bash
   sudo ausearch -m avc -ts recent
   ```

2. If the output shows a denial (e.g., `httpd` is denied access to a directory), it will look something like this:
   ```
   type=AVC msg=audit(1616599100.123:458): avc:  denied  { read } for  pid=2300 comm="httpd" name="myfile" dev="sda1" ino=12345 scontext=system_u:system_r:httpd_t:s0 tcontext=unconfined_u:object_r:default_t:s0 tclass=file
   ```

3. Identify the context causing the issue (in this case, `tcontext=default_t`).

4. Fix the context using `restorecon` or `chcon`, as discussed earlier:
   ```bash
   sudo restorecon -v /path/to/file
   ```

---


### **2. Viewing SELinux Contexts for Files and Processes**

#### Sample Question:
You need to check the SELinux context of a process and its associated files. Your task is to find the context of the `sshd` process and the `/etc/ssh/sshd_config` file.

#### Solution:
1. Check the SELinux context of the `sshd` process:
   ```bash
   ps -eZ | grep sshd
   ```
   Expected output (showing context):
   ```
   system_u:system_r:sshd_t:s0   1234 ? 00:00:00 sshd
   ```

2. Verify the SELinux context of the `/etc/ssh/sshd_config` file:
   ```bash
   ls -Z /etc/ssh/sshd_config
   ```
   Expected output:
   ```
   -rw-r--r--. root root system_u:object_r:sshd_config_t:s0 /etc/ssh/sshd_config
   ```

---

### **6. Modifying SELinux Booleans**

#### Sample Question 1:
Your task is to configure Apache (`httpd`) to serve user home directories. Enable the necessary SELinux Boolean to allow this, and ensure it persists across reboots.

#### Solution:
1. Check the status of the Boolean:
   ```bash
   getsebool httpd_enable_homedirs
   ```

2. Enable the Boolean:
   ```bash
   sudo setsebool -P httpd_enable_homedirs on
   ```

3. Verify the Boolean is set correctly:
   ```bash
   getsebool httpd_enable_homedirs
   ```
   Expected output:
   ```
   httpd_enable_homedirs --> on
   ```

---


### 4. **Manage SELinux port labels**

#### Sample Question:
You need to configure Apache to listen on port **8080** instead of the default port **80**. Update the SELinux port label to allow Apache to use this new port.

#### Solution:
1. Check which ports are currently allowed for Apache:
   ```bash
   sudo semanage port -l | grep http_port_t
   ```
   Example output:
   ```
   http_port_t    tcp    80, 443
   ```

2. Add port **8080** to the allowed list for the Apache service (`httpd`):
   ```bash
   sudo semanage port -a -t http_port_t -p tcp 8080
   ```

3. Verify the new port label has been applied:
   ```bash
   sudo semanage port -l | grep http_port_t
   ```
   Expected output:
   ```
   http_port_t    tcp    80, 443, 8080
   ```

4. Restart Apache to apply the changes (if required):
   ```bash
   sudo systemctl restart httpd
   ```


---

### 6. **Diagnose and address routine SELinux policy violations**

#### Sample Question:
An application running on your system is failing, and you suspect SELinux is blocking it. Investigate the SELinux logs and take action to resolve the issue.

#### Solution:
1. Check the SELinux audit logs for any **recent denials**:
   ```bash
   sudo ausearch -m avc -ts recent
   ```

2. Identify the specific denial. Example output:
   ```
   type=AVC msg=audit(1622123456.123:100): avc:  denied  { write } for pid=1234 comm="myapp" name="datafile" dev="sda1" ino=5678 scontext=system_u:system_r:myapp_t:s0 tcontext=unconfined_u:object_r:default_t:s0 tclass=file
   ```

3. In this case, the file has the wrong context (`default_t`). Fix it by restoring the default context:
   ```bash
   sudo restorecon -v /path/to/datafile
   ```

4. If you see that the issue is related to a Boolean, adjust the appropriate Boolean setting using `setsebool` as shown earlier.

---












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



# Temporarily change SELinux Status

- Use `Permissive` or `0` to put SELinux in permissive mode

- Use `Enforcing` or `1` to put SELinux in enforcing mode

       
## `setenforce`

Set to Permissive:

```bash
[bob@centos-host ~]$ sudo setenforce Permissive
```

```bash
[bob@centos-host ~]$ getenforce
Permissive
```

```bash
[bob@centos-host ~]$ sestatus
SELinux status:                 enabled
SELinuxfs mount:                /sys/fs/selinux
SELinux root directory:         /etc/selinux
Loaded policy name:             targeted
Current mode:                   permissive
Mode from config file:          enforcing
Policy MLS status:              enabled
Policy deny_unknown status:     allowed
Memory protection checking:     actual (secure)
Max kernel policy version:      32
```

Set to Enforcing:

```bash
[bob@centos-host ~]$ sudo setenforce Enforcing
```

```bash
[bob@centos-host ~]$ getenforce
Enforcing
```

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


# Permanently change SELinux Status

## `/etc/selinux/config`



```bash
sudo vi /etc/selinux/config
```

Then change SELINUX to one of these values:

- SELINUX=permissive

OR

- SELINUX=enforcing

OR

- SELINUX=disabled

Finally reboot the system to apply the changes

________________________________________________________________________________________________



#        `chcon` - change file SELinux security context (Temporarily)


Change the SELinux context of /var/index.html file to httpd_sys_content_t



```bash
[bob@centos-host ~]$ sudo chcon -t httpd_sys_content_t /var/index.html
```


```bash
[bob@centos-host ~]$ ls -Z /var/index.html

unconfined_u:object_r:httpd_sys_content_t:s0 /var/index.html
```


________________________________________________________________________________________________





## Modify SELinux policy to grant Apache HTTP Server access to files in /var/www/html/mydirectory.


1. Identify and Check Boolean:


## `getsebool -a`
## `semanage boolean -l`

```bash
getsebool httpd_read_user_content
```


2. Set Boolean (If it's off):  ( -P --> permanent )


## `setsebool -P httpd_read_user_content 1`

```bash
setsebool -P httpd_read_user_content 1
```




3. Verify Directory Context: (confirm the directory's SELinux context allows Apache access)

```bash
ls -Z /var/www/html/mydirectory
```



4. Restart Apache to apply changes:

```bash
systemctl restart httpd
```



5. Verify Access:

Create a test HTML file in mydirectory and access it in a browser to confirm Apache can now read files.



Key Points:

Enforcing Mode: These steps assume SELinux is in enforcing mode.

Disabling the Setting: Use "$ sudo setsebool -P httpd_read_user_content off".

Finding Booleans: (lists all booleans)

```bash
getsebool -a"
```

Lists HTTP-related booleans:

```bash
semanage boolean -l
```


Choosing Booleans: Base decisions on specific requirements.



Additional Considerations:

Context Troubleshooting: If necessary, use this command to restore the default SELinux context for the directory.

```bash
sudo restorecon -Rv /var/www/html/mydirectory"
```





________________________________________________________________________________________________


## Create a directory hierarchy /V1/V2/V3/, and recursively apply the SELinux context of the /etc directory.

## `semanage fcontext -a -t etc_t "/V1(/.*)?"`

## `restorecon -Rv /V1`


```bash
mkdir -p /V1/V2/V3

ls -dZ /etc

semanage fcontext -a -t etc_t "/V1(/.*)?"

restorecon -Rv /V1
```


________________________________________________________________________________________________

## Make the “httpd_t” domain permissive.

## `semanage permissive -a httpd_t`

```bash
semanage permissive -a httpd_t
```

________________________________________________________________________________________________


# to persistently set the bootloader to boot with selinux=0:
#
#    grubby --update-kernel ALL --args selinux=0
#
# To revert back to SELinux enabled:
#
#    grubby --update-kernel ALL --remove-args selinux
#


________________________________________________________________________________________________




