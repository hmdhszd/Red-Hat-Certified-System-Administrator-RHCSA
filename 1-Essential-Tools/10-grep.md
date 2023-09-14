
## `grep -i`

key `insensitive`

```bash
[bob@centos-host ~]$ grep centos /etc/os-release 
ID="centos"
CPE_NAME="cpe:/o:centos:centos:8"
HOME_URL="https://centos.org/"
```


```bash
[bob@centos-host ~]$ grep CentOS /etc/os-release 
NAME="CentOS Stream"
PRETTY_NAME="CentOS Stream 8"
REDHAT_SUPPORT_PRODUCT_VERSION="CentOS Stream"
```


```bash
[bob@centos-host ~]$ grep -i CentOS /etc/os-release 
NAME="CentOS Stream"
ID="centos"
PRETTY_NAME="CentOS Stream 8"
CPE_NAME="cpe:/o:centos:centos:8"
HOME_URL="https://centos.org/"
REDHAT_SUPPORT_PRODUCT_VERSION="CentOS Stream"
```

________________________________________________________________________________________________


## `grep -r`

search in all sub-directories and sub-files `recursively`

```bash
[bob@centos-host ~]$ sudo grep -r CentOS /etc/
/etc/yum.repos.d/CentOS-Stream-AppStream.repo:# CentOS-Stream-AppStream.repo
/etc/yum.repos.d/CentOS-Stream-AppStream.repo:# close to the client.  You should use this for CentOS updates unless you are
/etc/yum.repos.d/CentOS-Stream-AppStream.repo:name=CentOS Stream $releasever - AppStream
/etc/yum.repos.d/CentOS-Stream-BaseOS.repo:# CentOS-Stream-BaseOS.repo
/etc/yum.repos.d/CentOS-Stream-BaseOS.repo:# close to the client.  You should use this for CentOS updates unless you are
```

________________________________________________________________________________________________


## `grep -V`

`invert` match

```bash
[bob@centos-host ~]$ cat /etc/ssh/ssh_config | grep -V #
grep (GNU grep) 3.1
Copyright (C) 2017 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
```

________________________________________________________________________________________________


## `grep -w`

match `only` `words`

```bash
[bob@centos-host ~]$ grep -i 'red' /etc/os-release 
BUG_REPORT_URL="https://bugzilla.redhat.com/"
REDHAT_SUPPORT_PRODUCT="Red Hat Enterprise Linux 8"
REDHAT_SUPPORT_PRODUCT_VERSION="CentOS Stream"


[bob@centos-host ~]$ grep -iw 'red' /etc/os-release 
REDHAT_SUPPORT_PRODUCT="Red Hat Enterprise Linux 8"
```

________________________________________________________________________________________________


##  `grep -o`    --only-matching

it only shows the matching item, not the whole line

