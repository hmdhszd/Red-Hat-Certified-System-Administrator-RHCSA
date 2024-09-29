
## systemd.service

`startup` the `apps` and manage rerun them is the responsibility of `init system`

it consist of units (some text files that describe logics)


`unit` has various `types` such as, `Service`, `Socket`, `Device`, `Timer`,...


service units have instructions about : what command to issue to start a program, what to do if a program crashes or restarted,...

totally a `service unit` has info of how to `manage` the entire `lifecycle` of an `application`

```bash
man systemd.service
```

________________________________________________________________________________________________

## `systemctl cat sshd.service`

### see info of a service

```bash
[bob@centos-host ~]$ systemctl cat sshd.service

# /usr/lib/systemd/system/sshd.service
[Unit]
Description=OpenSSH server daemon
Documentation=man:sshd(8) man:sshd_config(5)
After=network.target sshd-keygen.target
Wants=sshd-keygen.target

[Service]
Type=notify
EnvironmentFile=-/etc/crypto-policies/back-ends/opensshserver.config
EnvironmentFile=-/etc/sysconfig/sshd
ExecStart=/usr/sbin/sshd -D $OPTIONS $CRYPTO_POLICY
ExecReload=/bin/kill -HUP $MAINPID
KillMode=process
Restart=on-failure
RestartSec=42s

[Install]
WantedBy=multi-user.target
```

________________________________________________________________________________________________

## `systemctl edit --full sshd.service`

edit service

```bash
[bob@centos-host ~]$ sudo systemctl edit --full sshd.service
```

## `systemctl revert sshd.service`

and to revert our changes:


```bash
[bob@centos-host ~]$ sudo systemctl revert sshd.service
```

________________________________________________________________________________________________


### status

```bash
[bob@centos-host ~]$ systemctl status sshd.service

● sshd.service - OpenSSH server daemon
   Loaded: loaded (/etc/systemd/system/sshd.service; enabled; vendor preset: enabled)
   Active: active (running) since Sun 2023-05-21 07:14:12 UTC; 5h 31min ago
     Docs: man:sshd(8)
           man:sshd_config(5)
 Main PID: 917 (sshd)
    Tasks: 1 (limit: 1340692)
   Memory: 12.2M
   CGroup: /system.slice/sshd.service
           └─917 /usr/sbin/sshd -D -oCiphers=aes256-gcm@openssh.com,chacha20-poly1305@op>
```

enabled

active (running)

Main PID: 917 (sshd)

loaded: full path of the service file

________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ systemctl enable sshd.service
```

________________________________________________________________________________________________


## `systemctl enable --now`

`enable` and `start`

```bash
[bob@centos-host ~]$ systemctl enable --now sshd.service
```

________________________________________________________________________________________________


## `systemctl disable --now`

`disable` and `stop`

```bash
[bob@centos-host ~]$ systemctl disable --now sshd.service
```

________________________________________________________________________________________________


## `systemctl is-enabled`

```bash
[bob@centos-host ~]$ systemctl is-enabled sshd.service
```

________________________________________________________________________________________________


## `systemctl is-active`

```bash
[bob@centos-host ~]$ systemctl is-active sshd.service
```

________________________________________________________________________________________________


```bash
[bob@centos-host ~]$ systemctl disable sshd.service
```

________________________________________________________________________________________________


```bash
[bob@centos-host ~]$ systemctl stop sshd.service
```

________________________________________________________________________________________________



```bash
[bob@centos-host ~]$ systemctl start sshd.service
```

________________________________________________________________________________________________



```bash
[bob@centos-host ~]$ systemctl restart sshd.service
```

________________________________________________________________________________________________



```bash
[bob@centos-host ~]$ systemctl reload sshd.service
```

## `systemctl reload-or-restart`

try to reload / restart if reload is not supported

```bash
[bob@centos-host ~]$ systemctl reload-or-restart sshd.service
```

________________________________________________________________________________________________


## `systemctl mask`

some of the services can start another services (even if we stopped them)

to disable them from starting other services, we can mask them

```bash
[bob@centos-host ~]$ systemctl mask atd.service
```

after that, if you wanna enable or start them, it wont work

## `systemctl unmask`


```bash
[bob@centos-host ~]$ systemctl unmask atd.service
```

________________________________________________________________________________________________


# to see all services of a system

## `systemctl list-units --type service --all`

```bash
[bob@centos-host ~]$ sudo systemctl list-units --type service --all

  UNIT                              LOAD      ACTIVE   SUB     DESCRIPTION              
● apparmor.service                  not-found inactive dead    apparmor.service         
● auditd.service                    not-found inactive dead    auditd.service           
  auth-rpcgss-module.service        loaded    inactive dead    Kernel Module supporting >
  banner.service                    loaded    inactive dead    "Applying banner for the >
● console-getty.service             masked    inactive dead    console-getty.service    
  dbus.service                      loaded    active   running D-Bus System Message Bus 
● display-manager.service           not-found inactive dead    display-manager.service  
```

________________________________________________________________________________________________

# `/usr/lib/systemd/`

`ExecStart` specifies the full `path` of a `command` that will be executed to `start` a service. 

`ExecReload` Specifies `commands` or `scripts` to be executed when the `unit is reloaded`. 


```bash
[bob@centos-host ~]$ cat /usr/lib/systemd/system/httpd.service

# See httpd.service(8) for more information on using the httpd service.

# Modifying this file in-place is not recommended, because changes
# will be overwritten during package upgrades.  To customize the
# behaviour, run "systemctl edit httpd" to create an override unit.

# For example, to pass additional options (such as -D definitions) to
# the httpd binary at startup, create an override unit (as is done by
# systemctl edit) and enter the following:

#       [Service]
#       Environment=OPTIONS=-DMY_DEFINE

[Unit]
Description=The Apache HTTP Server
Wants=httpd-init.service
After=network.target remote-fs.target nss-lookup.target httpd-init.service
Documentation=man:httpd.service(8)

[Service]
Type=notify
Environment=LANG=C

ExecStart=/usr/sbin/httpd $OPTIONS -DFOREGROUND
ExecReload=/usr/sbin/httpd $OPTIONS -k graceful
# Send SIGWINCH for graceful stop
KillSignal=SIGWINCH
KillMode=mixed
PrivateTmp=true

[Install]
WantedBy=multi-user.target
 
 ```
 



________________________________________________________________________________________________



# Configure “journald” to persist:

by default, the audit logs are temporary and are not being saved:

```bash
[root@centos-host bob]$ journalctl | grep "/log"
Mar 29 15:49:13 centos-host systemd-journald[318]: Runtime journal (/run/log/journal/2e407637679b477eb3e2a25b8ad9611d) is 820.0K, max 6.4M, 5.6M free.
```

#### Edit the configuration file for the systemd journal to make them persistent: `/etc/systemd/journald.conf`

```bash
[root@centos-host bob]$ vim /etc/systemd/journald.conf
```

#### Change `#Storage=auto` to `Storage=persistent`


#### Restart `systemd-journald`

```bash
[root@centos-host bob]$ systemctl restart systemd-journald.service

[root@centos-host bob]$ journalctl --flush
```

#### Now, we can see that they are saved in this path `/var/log/journal/`
```bash
[root@centos-host bob]$ journalctl | grep "/log"

Mar 29 16:53:39 centos-host systemd-journald[1727]: System journal (/var/log/journal/2e407637679b477eb3e2a25b8ad9611d) is 8.0M, max 4.0G, 3.9G free.
```



________________________________________________________________________________________________





