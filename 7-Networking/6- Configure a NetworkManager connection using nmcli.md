## Create a new connection profile named "myprofile1" for the enp0s3 device with the following static settings:

- Static IPv4 Address: 192.168.1.2/24

- Static IPv6 Address: fd01::101/64

- IPv4 Default Gateway: 192.168.1.1

- IPv6 Default Gateway: fd01::100

- IPv4 DNS Servers: 8.8.8.8, 8.8.4.4

- IPv6 DNS Server: fd01::111

- DNS Search Domain: example.com



________________________________________________________________________________________________

1. Create the connection:

```bash
nmcli connection add con-name myprofile1 ifname enp0s3 type ethernet
```

________________________________________________________________________________________________


2. Set IPv4 method to manual THEN set ip address: `ipv4.method` `ipv4.addresses`

```bash
 nmcli connection modify myprofile1 ipv4.method manual ipv4.addresses 192.168.1.2/24
```
________________________________________________________________________________________________


3. Set IPv6 method to manual THEN set ip address: `ipv6.method` `ipv6.addresses`

```bash
 nmcli connection modify myprofile1 ipv6.method manual ipv6.addresses fd01::101/64
```
________________________________________________________________________________________________

4. Set IPv4 gateways: `ipv4.gateway`

```bash
 nmcli connection modify myprofile1 ipv4.gateway 192.168.1.1
```
________________________________________________________________________________________________

5. Set IPv6 gateways: `ipv6.gateway`

```bash
 nmcli connection modify myprofile1 ipv6.gateway fd01::100
```
________________________________________________________________________________________________

6. Set IPv4 DNS servers:  `ipv4.dns`

dns should be in double quotation 

```bash
 nmcli connection modify myprofile1 ipv4.dns "8.8.8.8 8.8.4.4"
```
________________________________________________________________________________________________

7. Set IPv6 DNS servers: `ipv6.dns`

```bash
 nmcli connection modify myprofile1 ipv6.dns "fd01::111"
```

________________________________________________________________________________________________

8. Set IPv4 DNS search domain: `ipv4.dns-search`

```bash
 nmcli connection modify myprofile1 ipv4.dns-search example.com
```
________________________________________________________________________________________________

9. Set IPv6 DNS search domain: `ipv6.dns-search`

```bash
 nmcli connection modify myprofile1 ipv6.dns-search example.com
```
________________________________________________________________________________________________

10. Activate the profile:

```bash
 nmcli connection up myprofile1
 nmcli connection reload
 nmcli device reapply myprofile1
```

________________________________________________________________________________________________

### Verification:

```bash
nmcli device status

nmcli -p con show myprofile1

ip address show enp0s3

ip route

cat /etc/resolv.conf
```


________________________________________________________________________________________________

Notes:

RHEL 9 uses ‍`/etc/NetworkManager/system-connections/‍` for NetworkManager profiles, not `/etc/sysconfig/network-scripts/`.

________________________________________________________________________________________________

## Add secondary IPV4 and IPv6 addresses statically to the connection profile named "myprofile1" in a way that doesn’t compromise your existing settings:

IPV4 – 10.0.0.5/24

IPv6 – fd01::121/64

________________________________________________________________________________________________

1. Check the active network connection profiles to ensure the current state:

```bash
nmcli connection show --active

nmcli connection show myprofile1
```

________________________________________________________________________________________________

2. Configure a secondary IP address on "myprofile1": `+ipv4.addresses` `+ipv6.addresses`

```bash
nmcli con mod myprofile1 +ipv4.addresses 10.0.0.5/24

nmcli connection modify myprofile1 +ipv6.addresses fd01::121/64
```
________________________________________________________________________________________________

3. Apply the changes:

```bash
 nmcli connection up myprofile1
 nmcli connection reload
 nmcli device reapply myprofile1
```

________________________________________________________________________________________________

#### To confirm if IPv6 is enabled on your system, execute the following command:

```bash
sudo sysctl -a | grep ipv6.*disable
```

A value of 0 indicates that IPv6 is active on your system.


IF NOT, edit this file : `/etc/sysctl.conf` and put this value:

```bash
net.ipv6.conf.all.forwarding=1
```


________________________________________________________________________________________________
