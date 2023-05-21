


### Add route



```bash
[bob@centos-host ~]$ sudo ip route add 1.2.3.0/24 via 192.168.121.1
```

OR


```bash
[bob@centos-host ~]$ sudo ip route add 1.2.3.0/24 via 192.168.121.1 dev eth0
```

________________________________________________________________________________________________






```bash
[bob@centos-host ~]$ ip route | grep 1.2.3
1.2.3.0/24 via 192.168.121.1 dev eth0
```

________________________________________________________________________________________________




### Delete route

```bash
[bob@centos-host ~]$ sudo ip route del 1.2.3.0/24
```

________________________________________________________________________________________________




### Add default route

```bash
[bob@centos-host ~]$ sudo ip route add default via 192.168.121.1
```

________________________________________________________________________________________________


# these routes are temporary!

### ADD routes permanently

```bash
[bob@centos-host ~]$ nmcli connection show


first check the connection name:

NAME                UUID                                  TYPE      DEVICE  
System eth0         5fb06bd0-0bb0-7ffb-45f1-d6edd65f3e03  ethernet  eth0    
System eth1         9c92fad9-6ecb-3e6c-eb4d-8a47c6f50c04  ethernet  eth1    
```


then use the connection name:

```bash
[bob@centos-host ~]$ sudo nmcli connection modify System\ eth0 +ipv4.routes "1.2.3.0/24 192.168.121.1"
```



the apply the changes:

```bash
[bob@centos-host ~]$ sudo nmcli device reapply eth0
```

________________________________________________________________________________________________


### Remove routes permanently


```bash
[bob@centos-host ~]$ sudo nmcli connection modify System\ eth0 -ipv4.routes "1.2.3.0/24 192.168.121.1"
```

________________________________________________________________________________________________


we also can use nmtui to add / remove routes but at the end we should reapply!


