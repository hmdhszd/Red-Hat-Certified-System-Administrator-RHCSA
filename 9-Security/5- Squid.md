# Squid

### Squid is a Unix-based, specialized proxy server that acts as a caching proxy for web objects accessed through HTTP, HTTPS, FTP, and more.

### It is commonly used for several purposes, including caching, load balancing, `filtering traffic from websites`, and for `security` purposes.

________________________________________________________________________________________________

## Install

```bash
[bob@centos-host ~]$ sudo yum install -y squid
```

```bash
[bob@centos-host ~]$ sudo systemctl enable --now squid
```

________________________________________________________________________________________________


## Squid config file --> `/etc/squid/squid.conf`

________________________________________________________________________________________________


# 1- Create ACL:

add acl inside squid config file

```bash
acl test-acl src 100.64.0.0/10
acl test-acl port 443
acl test-acl src fc00::/7
```

________________________________________________________________________________________________

# 2- Add rule:

- ##  `deny` `incoming` `http access` to/from the IP/Port defined in the `ACL` called test-acl.


```bash
http_access deny test-acl
```

________________________________________________________________________________________________



- ##  `allow` `incoming` `http access` to/from the IP/Port defined in the `ACL` called test-acl.


```bash
http_access allow test-acl
```

________________________________________________________________________________________________


## add an ACL and `http_access` rule to `block` http traffic to `.facebook.com`.  ( `dstdomain` )


```bash
acl block-facebook-acl dstdomain .facebook.com
http_access deny block-facebook-acl
```

________________________________________________________________________________________________
