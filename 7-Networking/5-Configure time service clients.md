

## chronyd is used to adjust the system clock that runs in the kernel to synchronize with the NTP clock server.

________________________________________________________________________________________________







```bash
[bob@centos-host ~]$ dns install chrony
```

________________________________________________________________________________________________






```bash
[bob@centos-host ~]$ systemctl enable --now chronyd.service
```

________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ systemctl status chronyd.service

● chronyd.service - NTP client/server
   Loaded: loaded (/usr/lib/systemd/system/chronyd.service; enabled; vendor preset: enab>
   Active: active (running) since Sun 2023-05-21 19:17:07 UTC; 1h 14min ago
     Docs: man:chronyd(8)
           man:chrony.conf(5)
 Main PID: 645 (chronyd)
    Tasks: 1 (limit: 5970)
   Memory: 1.6M
   CGroup: /system.slice/chronyd.service
           └─645 /usr/sbin/chronyd
```

________________________________________________________________________________________________


check info of time zone  of the server

```bash
[bob@centos-host ~]$ timadatectl
```

________________________________________________________________________________________________


### Set timezone

```bash
[bob@centos-host ~]$ sudo timedatectl set-timezone "America/Toronto"
```

________________________________________________________________________________________________


### Enable time synchronization 

```bash
[bob@centos-host ~]$ sudo systemctl set-ntp true
```

________________________________________________________________________________________________
