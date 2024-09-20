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

3. **SELinux Booleans**:
   Booleans are used to toggle specific features of SELinux on and off, allowing more granular control without changing the SELinux policy.

4. **SELinux Policies**:
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

#### 6. **Changing File Contexts**

You can change the SELinux context of a file using `chcon`. For example, if a file needs to be accessed by the `httpd` service, you can set its type to `httpd_sys_content_t`:
```bash
sudo chcon -t httpd_sys_content_t /var/www/html/myfile.html
```

To make changes permanent, use the `semanage` command to modify the policy:
```bash
sudo semanage fcontext -a -t httpd_sys_content_t "/var/www/html(/.*)?"
```
Then, apply the context with `restorecon`:
```bash
sudo restorecon -R /var/www/html
```

#### 7. **Modifying SELinux Booleans**

SELinux Booleans can be toggled to adjust system behavior. For example, to allow the Apache server to connect to the network, you can enable the `httpd_can_network_connect` Boolean:
```bash
sudo setsebool -P httpd_can_network_connect on
```
The `-P` option makes the change persistent across reboots.

To list all available SELinux Booleans:
```bash
sudo getsebool -a
```

To check the value of a specific Boolean:
```bash
sudo getsebool httpd_can_network_connect
```

---

### Example RHCSA Exam Tasks Involving SELinux

Here are common tasks you may encounter in the exam:

1. **Task**: You are asked to troubleshoot why a web server isn’t able to access a file in `/var/www/html`.
   - **Solution**: Check file permissions, and then check the SELinux context using `ls -Z`. If the context is wrong, fix it using `restorecon` or `chcon`.

2. **Task**: You are instructed to allow an Apache web server to connect to a backend database server over the network.
   - **Solution**: Enable the `httpd_can_network_connect` Boolean:
     ```bash
     sudo setsebool -P httpd_can_network_connect on
     ```

3. **Task**: Temporarily switch SELinux to permissive mode for troubleshooting.
   - **Solution**:
     ```bash
     sudo setenforce 0
     ```

---

### Conclusion

SELinux is an important part of the RHCSA exam, and mastering the basic commands and concepts will help you secure higher marks. Focus on understanding:
- SELinux modes (enforcing, permissive, disabled)
- Viewing and modifying file/process contexts
- Using `restorecon`, `chcon`, `setsebool`, and related commands to solve typical system administration tasks.

Let me know if you'd like to go deeper into specific commands or more complex examples!







Here are **realistic RHCSA exam-style sample questions** for each important part of SELinux. These questions are designed to simulate what you may encounter in the exam.

---

### **1. SELinux Modes: Enforcing, Permissive, Disabled**

#### Sample Question:
Your task is to temporarily change the SELinux mode to permissive to troubleshoot an issue, and then return it to enforcing mode afterward.

#### Solution:
1. Check current SELinux mode:
   ```bash
   sestatus
   ```
2. Set SELinux to permissive mode (temporary change):
   ```bash
   sudo setenforce 0
   ```
3. Verify the change:
   ```bash
   sestatus
   ```
4. After troubleshooting, switch back to enforcing mode:
   ```bash
   sudo setenforce 1
   ```
5. Confirm that SELinux is back to enforcing mode:
   ```bash
   sestatus
   ```

#### Notes:
- **Temporary changes** only last until a reboot. 
- You can modify the mode permanently by editing `/etc/selinux/config`.

---

### **2. Viewing SELinux Contexts (Files and Processes)**

#### Sample Question:
A new HTML file, `/var/www/html/home.html`, has been created for your Apache web server, but the web server is unable to serve this file. Verify the file's SELinux context and correct any issues if necessary.

#### Solution:
1. Check the SELinux context of the file:
   ```bash
   ls -Z /var/www/html/home.html
   ```
   You might see:
   ```
   -rw-r--r--. root root unconfined_u:object_r:default_t:s0 home.html
   ```
   The type `default_t` indicates the wrong context.

2. Reset the correct context using `restorecon`:
   ```bash
   sudo restorecon -v /var/www/html/home.html
   ```

3. Verify that the context is correct now (it should be `httpd_sys_content_t`):
   ```bash
   ls -Z /var/www/html/home.html
   ```
   Correct output:
   ```
   -rw-r--r--. root root unconfined_u:object_r:httpd_sys_content_t:s0 home.html
   ```

---

### **3. Fixing SELinux Contexts Using `restorecon`**

#### Sample Question:
A user reports that after copying a backup file `/home/user/backup.tar.gz` to `/var/www/html/`, the Apache web server cannot access it. Restore the appropriate SELinux context for this file.

#### Solution:
1. Verify the file’s current SELinux context:
   ```bash
   ls -Z /var/www/html/backup.tar.gz
   ```
   You may see:
   ```
   -rw-r--r--. root root unconfined_u:object_r:user_home_t:s0 backup.tar.gz
   ```

2. Fix the context using `restorecon`:
   ```bash
   sudo restorecon -v /var/www/html/backup.tar.gz
   ```

3. Verify the new SELinux context (it should now be `httpd_sys_content_t`):
   ```bash
   ls -Z /var/www/html/backup.tar.gz
   ```
   Correct output:
   ```
   -rw-r--r--. root root unconfined_u:object_r:httpd_sys_content_t:s0 backup.tar.gz
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

### **5. Making Permanent SELinux Context Changes Using `semanage`**

#### Sample Question:
You need to make the SELinux context of the `/srv/myapp/` directory and all its subdirectories/files permanently set to allow Apache (`httpd`) to access them.

#### Solution:
1. Add a permanent context rule using `semanage`:
   ```bash
   sudo semanage fcontext -a -t httpd_sys_content_t "/srv/myapp(/.*)?"
   ```

2. Apply the context changes to existing files:
   ```bash
   sudo restorecon -R /srv/myapp
   ```

3. Verify that the correct context is applied:
   ```bash
   ls -Z /srv/myapp/
   ```
   Example output:
   ```
   -rw-r--r--. root root unconfined_u:object_r:httpd_sys_content_t:s0 run.sh
   ```

#### Notes:
- The `semanage fcontext` command ensures the context persists across reboots or relabels.
- Use `restorecon -R` to apply the context to existing files.

---

### **6. Modifying SELinux Booleans**

#### Sample Question:
You are required to configure your Apache web server so it can communicate with an external database over the network. Enable the necessary SELinux Boolean to allow this.

#### Solution:
1. Check the current status of the `httpd_can_network_connect` Boolean:
   ```bash
   getsebool httpd_can_network_connect
   ```

2. Enable the Boolean to allow network connections:
   ```bash
   sudo setsebool -P httpd_can_network_connect on
   ```

3. Verify the change:
   ```bash
   getsebool httpd_can_network_connect
   ```

   You should see:
   ```
   httpd_can_network_connect --> on
   ```

#### Notes:
- The `-P` flag makes the Boolean change persistent across reboots.
- **Common SELinux Booleans** you may need to modify in the exam:
  - `httpd_can_network_connect`: Allows Apache to make network connections.
  - `httpd_enable_homedirs`: Allows Apache to serve content from user home directories.
  - `httpd_can_sendmail`: Allows Apache to send emails using `sendmail`.

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



---

### **1. SELinux Modes: Enforcing, Permissive, Disabled**

#### Sample Question:
You are troubleshooting an application and need to disable SELinux temporarily for testing purposes. After your troubleshooting is complete, you must re-enable SELinux. Demonstrate how you would disable SELinux temporarily, verify the change, and then re-enable it.

#### Solution:
1. Check current SELinux mode:
   ```bash
   sestatus
   ```

2. Temporarily disable SELinux by setting it to permissive:
   ```bash
   sudo setenforce 0
   ```

3. Verify SELinux is in permissive mode:
   ```bash
   sestatus
   ```

4. Re-enable SELinux by setting it to enforcing:
   ```bash
   sudo setenforce 1
   ```

5. Verify the mode is back to enforcing:
   ```bash
   sestatus
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

### **3. Fixing SELinux Contexts Using `restorecon`**

#### Sample Question:
The `/var/www/html/index.html` file has been copied from another location, but it cannot be accessed by the Apache web server. Restore the correct SELinux context so that the web server can access this file.

#### Solution:
1. Check the current context of the file:
   ```bash
   ls -Z /var/www/html/index.html
   ```
   Example output showing incorrect context:
   ```
   -rw-r--r--. root root unconfined_u:object_r:default_t:s0 /var/www/html/index.html
   ```

2. Restore the correct context for this file:
   ```bash
   sudo restorecon -v /var/www/html/index.html
   ```

3. Verify the new SELinux context (should be `httpd_sys_content_t`):
   ```bash
   ls -Z /var/www/html/index.html
   ```
   Expected output:
   ```
   -rw-r--r--. root root unconfined_u:object_r:httpd_sys_content_t:s0 /var/www/html/index.html
   ```

---

### **4. Changing SELinux Contexts Using `chcon`**

#### Sample Question:
You have a custom script `/opt/myapp/start.sh` that should be executed by the Apache server (`httpd`). The current SELinux context prevents it from running. Modify the context so that the script can be executed by Apache.

#### Solution:
1. Check the current SELinux context of the file:
   ```bash
   ls -Z /opt/myapp/start.sh
   ```

2. Modify the context so that it allows Apache to execute the script:
   ```bash
   sudo chcon -t httpd_sys_script_exec_t /opt/myapp/start.sh
   ```

3. Verify the new context:
   ```bash
   ls -Z /opt/myapp/start.sh
   ```

---

### **5. Making Permanent SELinux Context Changes Using `semanage`**

#### Sample Question:
You are asked to make sure the `/opt/myapp/` directory and all future files created in it are permanently set with the `httpd_sys_content_t` context to allow access by Apache. Ensure the change persists across reboots.

#### Solution:
1. Add the permanent context rule using `semanage`:
   ```bash
   sudo semanage fcontext -a -t httpd_sys_content_t "/opt/myapp(/.*)?"
   ```

2. Apply the context to existing files and directories:
   ```bash
   sudo restorecon -R /opt/myapp
   ```

3. Verify the context:
   ```bash
   ls -Z /opt/myapp
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

#### Sample Question 2:
You are tasked with allowing Apache (`httpd`) to make outgoing network connections. Enable the appropriate SELinux Boolean and make sure the change is persistent across reboots.

#### Solution:
1. Check the current status of the Boolean:
   ```bash
   getsebool httpd_can_network_connect
   ```

2. Enable the Boolean:
   ```bash
   sudo setsebool -P httpd_can_network_connect on
   ```

3. Verify the change:
   ```bash
   getsebool httpd_can_network_connect
   ```
   Expected output:
   ```
   httpd_can_network_connect --> on
   ```

---

### **7. Troubleshooting SELinux Issues Using Audit Logs**

#### Sample Question:
An application is not working, and you suspect SELinux is blocking it. Investigate the SELinux audit logs for the last denial and take appropriate action to fix the issue.

#### Solution:
1. Check the SELinux denial logs using `ausearch`:
   ```bash
   sudo ausearch -m avc -ts recent
   ```

2. Identify the denial in the output (look for lines that contain `denied`). For example, you may find:
   ```
   type=AVC msg=audit(1622123456.123:100): avc:  denied  { write } for pid=1234 comm="myapp" name="datafile" dev="sda1" ino=5678 scontext=system_u:system_r:myapp_t:s0 tcontext=unconfined_u:object_r:default_t:s0 tclass=file
   ```

3. Based on the output, you see the `tcontext=default_t` which indicates an incorrect file context. Fix it using `restorecon`:
   ```bash
   sudo restorecon -v /path/to/datafile
   ```

4. Re-run the application to check if the issue is resolved.

---

### **8. Disabling SELinux for Specific Services**

#### Sample Question:
A new service is being installed, but it is not working due to SELinux. Your task is to temporarily disable SELinux for this service without turning off SELinux completely for the system.

#### Solution:
1. Identify the service and the context it is running under by using `ps -eZ` or checking its log files.

2. Set the service to be unconfined by changing its context. Use `chcon` or `semanage` if necessary, to modify the process or its configuration files.

3. Make sure to document and verify the changes using `semanage` or `setsebool`, but be mindful that this approach may depend on specific service needs. Some services might need a combination of **Booleans** or **custom policies** for proper operation.

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



