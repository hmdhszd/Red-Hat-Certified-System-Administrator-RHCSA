

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


## `nmcli`

```bash
[bob@centos-host ~]$ nmcli

eth0: connected to System eth0
        "Red Hat Virtio"
        ethernet (virtio_net), 52:54:00:3A:D9:B6, hw, mtu 1500
        ip4 default
        inet4 192.168.121.190/24
        route4 0.0.0.0/0
        route4 192.168.121.0/24
        inet6 fe80::5054:ff:fe3a:d9b6/64
        route6 fe80::/64
        route6 ff00::/8

eth1: connected to System eth1
        "Red Hat Virtio"
        ethernet (virtio_net), 52:54:00:2D:B6:AB, hw, mtu 1500
        inet4 172.28.128.137/24
        route4 0.0.0.0/0
        route4 172.28.128.0/24
        inet6 fe80::5054:ff:fe2d:b6ab/64
        route6 ff00::/8
        route6 fe80::/64

docker0: connected (externally) to docker0
        "docker0"
        bridge, 02:42:9F:29:E3:4C, sw, mtu 1500
        inet4 172.17.0.1/16
        route4 172.17.0.0/16

lo: unmanaged
        "lo"
        loopback (unknown), 00:00:00:00:00:00, sw, mtu 65536

DNS configuration:
        servers: 192.168.121.1
        interface: eth0

        servers: 172.28.128.1
        interface: eth1

```

________________________________________________________________________________________________




## `nmcli connection show`

these are the items that we can modify:

```bash
[bob@centos-host ~]$ nmcli connection show System\ eth1

connection.id:                          System eth1
connection.uuid:                        9c92fad9-6ecb-3e6c-eb4d-8a47c6f50c04
connection.stable-id:                   --
connection.type:                        802-3-ethernet
connection.interface-name:              eth1
connection.autoconnect:                 yes
connection.autoconnect-priority:        0
connection.autoconnect-retries:         -1 (default)
connection.multi-connect:               0 (default)
connection.auth-retries:                -1
connection.timestamp:                   1704575177
connection.read-only:                   no
connection.permissions:                 --
connection.zone:                        --
connection.master:                      --
connection.slave-type:                  --
connection.autoconnect-slaves:          -1 (default)
connection.secondaries:                 --
connection.gateway-ping-timeout:        0
connection.metered:                     unknown
connection.lldp:                        default
connection.mdns:                        -1 (default)
connection.llmnr:                       -1 (default)
connection.wait-device-timeout:         -1
802-3-ethernet.port:                    --
802-3-ethernet.speed:                   0
802-3-ethernet.duplex:                  --
802-3-ethernet.auto-negotiate:          no
.
.
.
```

________________________________________________________________________________________________


## Add a new connection

## `nmcli connection add con-name type ifname`

```bash
[bob@centos-host ~]$ sudo nmcli connection add con-name "my-connection" type ethernet ifname eth1
Connection 'my-connection' (74015611-557e-4829-a1b7-70f9be7e509f) successfully added.
```


## Add a new connection with IP and Gateway

## `nmcli connection add con-name type ifname ip4 gw4`

```bash
[bob@centos-host ~]$ sudo nmcli connection add con-name "my-connection-2" type ethernet ifname eth1 ip4 1.2.3.4/24 gw4 8.8.8.8
Connection 'my-connection-2' (e7833ecb-db54-4720-9d8b-acd4ab411ac5) successfully added.
```

________________________________________________________________________________________________


Verify:

you can see that it's not still up

```bash
[bob@centos-host ~]$ nmcli connection
NAME                UUID                                  TYPE      DEVICE  
System eth0         5fb06bd0-0bb0-7ffb-45f1-d6edd65f3e03  ethernet  eth0    
System eth1         9c92fad9-6ecb-3e6c-eb4d-8a47c6f50c04  ethernet  eth1    
docker0             8f2a46ca-79f9-4af6-a72e-11316d785276  bridge    docker0 
ens3                814f575f-6d69-42a1-a7da-3658c903dffa  ethernet  --      
my-connection       74015611-557e-4829-a1b7-70f9be7e509f  ethernet  --      
my-connection-2     e7833ecb-db54-4720-9d8b-acd4ab411ac5  ethernet  --      
Wired connection 1  a786ea47-c4a4-3375-997b-7c67a864329c  ethernet  --
```

________________________________________________________________________________________________


## `nmcli connection up`

```bash
[bob@centos-host ~]$ sudo nmcli connection up my-connection

Connection successfully activated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/4)
```

```bash
[bob@centos-host ~]$ nmcli connection

NAME                UUID                                  TYPE      DEVICE  
System eth0         5fb06bd0-0bb0-7ffb-45f1-d6edd65f3e03  ethernet  eth0    
my-connection       74015611-557e-4829-a1b7-70f9be7e509f  ethernet  eth1    
docker0             8f2a46ca-79f9-4af6-a72e-11316d785276  bridge    docker0 
ens3                814f575f-6d69-42a1-a7da-3658c903dffa  ethernet  --      
my-connection-2     e7833ecb-db54-4720-9d8b-acd4ab411ac5  ethernet  --      
System eth1         9c92fad9-6ecb-3e6c-eb4d-8a47c6f50c04  ethernet  --      
Wired connection 1  a786ea47-c4a4-3375-997b-7c67a864329c  ethernet  --      
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
