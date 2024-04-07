

# Modify SELINUX at boot time:

we can boot system into permisive mode:

at the boot time press `e`, and after "quit", we enter this:

```bash
enforcing=0
```

OR

we can make the system not to load anything related to selinux:


```bash
selinux=0
```

OR

it will autorelabel files at the next boot:

```bash
autorelabel=1
```

________________________________________________________________________________________________



# Diagnose and address routine selinux policy violations:

to see the logs of the policy violations:

```bash
journalctl -xe
```

then you will find this command in the audit logs, run the command:

```bash
ausearch -c 'httpd' --raw | audit2allow -M my-httpd
```
then

```bash
semodule -i my-httpd.pp
```
________________________________________________________________________________________________


# Change HTTPD (Apache) root directory


- Create the new default root directory:

```bash
mkdir /my-directory
echo hello > /my-directory/index.html
```


- Change config file:

```bash
/etc/httpd/conf/httpd.conf

DocumentRoot /my-directory

<Directory /my-directory>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
        Allow from all
</Directory>
```


- Change SELINUX Context:

```bash
semanage fcontext -a -t httpd_sys_content_d "/my-directory(/.*)?"

restorecon -Rv /my-directory
```


________________________________________________________________________________________________




```bash

```

________________________________________________________________________________________________




```bash

```

________________________________________________________________________________________________
