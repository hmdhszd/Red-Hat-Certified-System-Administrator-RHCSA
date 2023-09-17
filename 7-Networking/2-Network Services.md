

## NetworkManager


```bash
[bob@centos-host ~]$ sudo dnf install NetworkManager
```

________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ sudo systemctl enable --now  NetworkManager.service
```

________________________________________________________________________________________________






```bash
[bob@centos-host ~]$ systemctl status NetworkManager.service

● NetworkManager.service - Network Manager
   Loaded: loaded (/usr/lib/systemd/system/NetworkManager.service; enabled; vendor prese>
   Active: active (running) since Sun 2023-05-21 17:45:06 UTC; 13s ago
     Docs: man:NetworkManager(8)
 Main PID: 3782 (NetworkManager)
    Tasks: 4 (limit: 1340692)
   Memory: 5.9M
   CGroup: /system.slice/NetworkManager.service
           └─3782 /usr/sbin/NetworkManager --no-daemon
```

________________________________________________________________________________________________



## `nmcli connection show`

`list` the `interfaces`

```bash
[bob@centos-host ~]$ nmcli connection show

NAME                UUID                                  TYPE      DEVICE 
eth0                8c678a2f-7d2d-42d3-88bf-e6a44b0b0177  ethernet  eth0   
eth1                47a00e59-8305-4ba3-afc2-d4b1a3e487ab  ethernet  eth1   
virbr0              bc41bef5-991e-45e5-8d00-c2bb0111eee5  bridge    virbr0 
Wired connection 1  a57a47fb-baf6-3c00-adda-c66ead0630e6  ethernet  --     
Wired connection 2  524c655b-2df1-3e4c-9038-d45b2c1ba4f8  ethernet  --    
```

________________________________________________________________________________________________

## `nmcli connection modify`

initialize a connection at the boot time (`autoconnect`)

```bash
[bob@centos-host ~]$ sudo nmcli connection modify enp0s3 autoconnect yes
```

________________________________________________________________________________________________


## `ss -tulpn`

### to see what programs are running and waiting for `incoming network connections` (open ports) --> `ss` and `netstat`

(always use with sudo)

```bash
[bob@centos-host ~]$ sudo ss -tulpn

Netid    State     Recv-Q    Send-Q        Local Address:Port        Peer Address:Port   Process                                                                                  
udp      UNCONN    0         0             192.168.122.1:53               0.0.0.0:*       users:(("dnsmasq",pid=1262,fd=5))                                                       
. . .
```

the service that is listening on port 53    -->   dnsmasq

the process ID of the service               -->   1262


________________________________________________________________________________________________


## `ps`


check the process of the previous service


```bash
[bob@centos-host ~]$ ps 1262

    PID TTY      STAT   TIME COMMAND
   1262 ?        S      0:00 /usr/sbin/dnsmasq --conf-file=/var/lib/libvirt/dnsmasq/defau
```

________________________________________________________________________________________________


## `lsof -p`

see the files that this process is using

```bash
[bob@centos-host ~]$ lsof -p 1262

COMMAND  PID    USER   FD      TYPE DEVICE SIZE NODE NAME
dnsmasq 1262 dnsmasq  cwd   unknown                  /proc/1262/cwd (readlink: Permission denied)
dnsmasq 1262 dnsmasq  rtd   unknown                  /proc/1262/root (readlink: Permission denied)
dnsmasq 1262 dnsmasq  txt   unknown                  /proc/1262/exe (readlink: Permission denied)
dnsmasq 1262 dnsmasq NOFD                            /proc/1262/fd (opendir: Permission denied)
```

________________________________________________________________________________________________



## `netstat -tulpn`


```bash
[bob@centos-host ~]$ sudo netstat -tulpn
```

________________________________________________________________________________________________
