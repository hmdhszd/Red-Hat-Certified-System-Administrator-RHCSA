

### IP 


```bash
[bob@centos-host ~]$ ip a

1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: virbr0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default qlen 1000
    link/ether 52:54:00:7e:33:36 brd ff:ff:ff:ff:ff:ff
    inet 192.168.122.1/24 brd 192.168.122.255 scope global virbr0
       valid_lft forever preferred_lft forever
9503: eth0@if9504: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1450 qdisc noqueue state UP group default 
    link/ether 02:42:c0:18:9c:06 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.24.156.6/24 brd 192.24.156.255 scope global eth0
       valid_lft forever preferred_lft forever
9505: eth1@if9506: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:ac:19:00:34 brd ff:ff:ff:ff:ff:ff link-netnsid 1
    inet 172.25.0.52/24 brd 172.25.0.255 scope global eth1
       valid_lft forever preferred_lft forever
```

________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ ip l

1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: virbr0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN mode DEFAULT group default qlen 1000
    link/ether 52:54:00:7e:33:36 brd ff:ff:ff:ff:ff:ff
9503: eth0@if9504: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1450 qdisc noqueue state UP mode DEFAULT group default 
    link/ether 02:42:c0:18:9c:06 brd ff:ff:ff:ff:ff:ff link-netnsid 0
9505: eth1@if9506: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DEFAULT group default 
    link/ether 02:42:ac:19:00:34 brd ff:ff:ff:ff:ff:ff link-netnsid 1
```

________________________________________________________________________________________________






```bash
[bob@centos-host ~]$ ip r

default via 172.25.0.1 dev eth1 
172.25.0.0/24 dev eth1 proto kernel scope link src 172.25.0.52 
192.24.156.0/24 dev eth0 proto kernel scope link src 192.24.156.6 
192.168.122.0/24 dev virbr0 proto kernel scope link src 192.168.122.1 linkdown 
```

________________________________________________________________________________________________


### DNS

```bash
[bob@centos-host ~]$ cat /etc/resolv.conf

search us-central1-a.c.kk-lab-prod.internal c.kk-lab-prod.internal google.internal
nameserver 172.25.0.1
options ndots:0
```

________________________________________________________________________________________________


### interface configuration

```bash
[bob@centos-host ~]$ ls /etc/sysconfig/network-scripts/
```

________________________________________________________________________________________________


### config network with GUI

```bash
[bob@centos-host ~]$ sudo nmtui
```

to apply the changes:

```bash
[bob@centos-host ~]$ sudo nmcli device reapply enp0s 3
```

________________________________________________________________________________________________


### map hostname to IP addresse statically

```bash
[bob@centos-host ~]$ cat /etc/hosts

127.0.0.1       localhost
::1     localhost ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
192.24.156.6    centos-host
```

________________________________________________________________________________________________
