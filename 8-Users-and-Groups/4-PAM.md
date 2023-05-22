


Linux Pluggable Authentication Modules is a suite of libraries that allows a Linux system administrator to configure methods to authenticate users.

It provides a flexible and centralized way to switch authentication methods for secured applications by using configuration files instead of changing application code.

________________________________________________________________________________________________


PAM files


```bash
[bob@centos-host ~]$ ls /etc/pam.d/

config-util       passwd         remote          sshd  sudo-i
fingerprint-auth  password-auth  runuser         su    system-auth
login             polkit-1       runuser-l       su-l  systemd-user
other             postlogin      smartcard-auth  sudo  vlock
```

________________________________________________________________________________________________



PAM config of "su" utility


```bash
[bob@centos-host ~]$ sudo cat /etc/pam.d/su

#%PAM-1.0
auth            required        pam_env.so
auth            sufficient      pam_rootok.so
# Uncomment the following line to implicitly trust users in the "wheel" group.
#auth           sufficient      pam_wheel.so trust use_uid
# Uncomment the following line to require a user to be in the "wheel" group.
#auth           required        pam_wheel.so use_uid
auth            substack        system-auth
auth            include         postlogin
account         sufficient      pam_succeed_if.so uid = 0 use_uid quiet
account         include         system-auth
password        include         system-auth
session         include         system-auth
session         include         postlogin
session         optional        pam_xauth.so
```

________________________________________________________________________________________________





```bash
[bob@centos-host ~]$ man pam.conf
```

________________________________________________________________________________________________


In the "/etc/pam.d/su" file in Red Hat Linux, each line consists of several columns that define the configuration for the Pluggable Authentication Modules (PAM) related to the "su" command. Let's break down the columns and their possible values:

Column 1: The control flag

Possible values: "auth," "account," "password," or "session"

Specifies the type of PAM stack that the following module will be applied to.

Column 2: The module type



Possible values: Various PAM module names

Indicates the PAM module to be used for the specified stack.

Column 3: The control flag modifier


Possible values: "required," "requisite," "sufficient," or "optional"

Modifies the behavior of the module in relation to the success or failure of the preceding modules in the stack.

Column 4: The module options


Possible values: Module-specific options

Specifies additional parameters or configuration options for the PAM module.

The columns are typically separated by whitespace.


Note: The specific values in each column can vary depending on the system configuration and the specific PAM modules installed.

It's important to consult the system documentation or the comments within the file itself for precise details about the values used in a particular "/etc/pam.d/su" file.







________________________________________________________________________________________________





```bash

```

________________________________________________________________________________________________






```bash

```

________________________________________________________________________________________________





```bash

```

________________________________________________________________________________________________





```bash

```

________________________________________________________________________________________________





```bash

```

________________________________________________________________________________________________






```bash

```

________________________________________________________________________________________________





```bash

```

________________________________________________________________________________________________






```bash

```

________________________________________________________________________________________________





```bash

```

________________________________________________________________________________________________





```bash

```

________________________________________________________________________________________________





```bash

```

________________________________________________________________________________________________






```bash

```

________________________________________________________________________________________________





```bash

```

________________________________________________________________________________________________





```bash

```

________________________________________________________________________________________________





```bash

```

________________________________________________________________________________________________





```bash

```

________________________________________________________________________________________________





```bash

```

________________________________________________________________________________________________






```bash

```

________________________________________________________________________________________________





```bash

```

________________________________________________________________________________________________





```bash

```

________________________________________________________________________________________________





```bash

```

________________________________________________________________________________________________






```bash

```

________________________________________________________________________________________________





```bash

```

________________________________________________________________________________________________





```bash

```

________________________________________________________________________________________________





```bash

```

________________________________________________________________________________________________






```bash

```

________________________________________________________________________________________________





```bash

```

________________________________________________________________________________________________






```bash

```

________________________________________________________________________________________________





```bash

```

________________________________________________________________________________________________





```bash

```

________________________________________________________________________________________________





```bash

```

________________________________________________________________________________________________






```bash

```

________________________________________________________________________________________________





```bash

```

________________________________________________________________________________________________





```bash

```

________________________________________________________________________________________________





```bash

```

________________________________________________________________________________________________
