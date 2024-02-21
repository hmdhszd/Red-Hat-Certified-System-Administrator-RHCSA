

## `chronyd` is used to adjust the `system clock` that runs in the kernel to `synchronize` with the `NTP` clock server.

________________________________________________________________________________________________







```bash
[bob@centos-host ~]$ dns install chrony
```

________________________________________________________________________________________________






```bash
[bob@centos-host ~]$ systemctl enable --now chronyd.service
```

________________________________________________________________________________________________


## `systemctl status chronyd.service`


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





## Add a time server ip to Chrony configuration:

```bash
nano /etc/chrony.conf
```

we should `comment` the line: pool 2.centos.pool.ntp.org iburst

and add the server ip address:


```bash
server 192.75.96.196 iburst
```

after changing the configuration, we should restart the Chronyd service:


```bash
sudo systemctl restart chronyd
```


Verify the ntp server:



```bash
[bob@centos-host ~]$ chronyc sources

MS Name/IP address         Stratum Poll Reach LastRx Last sample               
===============================================================================
^? 192.75.96.196                 0   7     0     -     +0ns[   +0ns] +/-    0ns```
```

________________________________________________________________________________________________

then we check if the `NTP` service is `active` or `inactive`



```bash
[bob@centos-host ~]$ sudo timedatectl status

               Local time: Sat 2023-12-23 20:43:32 UTC
           Universal time: Sat 2023-12-23 20:43:32 UTC
                 RTC time: Sat 2023-12-23 20:43:31
                Time zone: UTC (UTC, +0000)
System clock synchronized: yes
              NTP service: inactive
          RTC in local TZ: no
```

________________________________________________________________________________________________


if it was inactive, the we should active it


`Enable` time `synchronization` 

## `timedatectl set-ntp true`

```bash
[bob@centos-host ~]$ sudo timedatectl set-ntp true
```



## `timedatectl`

check info of time zone  of the server

```bash
[bob@centos-host ~]$ timedatectl

               Local time: Mon 2023-05-22 02:28:55 IST
           Universal time: Sun 2023-05-21 20:58:55 UTC
                 RTC time: Sun 2023-05-21 20:58:55
                Time zone: Asia/Kolkata (IST, +0530)
System clock synchronized: yes
              NTP service: active
          RTC in local TZ: no
```


________________________________________________________________________________________________


## `timedatectl list-timezones`


```bash
[bob@centos-host ~]$ timedatectl list-timezones 
```

________________________________________________________________________________________________



## `timedatectl set-timezone`

```bash
[bob@centos-host ~]$ sudo timedatectl set-timezone "America/Toronto"
```


________________________________________________________________________________________________


## `timedatectl set-local-rtc 1`

Set system to use RTC

```bash
[bob@centos-host ~]$ sudo timedatectl set-local-rtc 1
```

________________________________________________________________________________________________
