


login to your subscription

```bash
[bob@centos-host ~]$ sudo subscription-manager register --username XXXXXXX --password YYYYYYY
```

________________________________________________________________________________________________


attach the machine to your subscription

```bash
[bob@centos-host ~]$ sudo subscription-manager attach --auto
```

________________________________________________________________________________________________


see list of repo available on your machine

```bash
[bob@centos-host ~]$ sudo yum repolist

repo id                        repo name
appstream                      CentOS Stream 8 - AppStream
baseos                         CentOS Stream 8 - BaseOS
extras                         CentOS Stream 8 - Extras
extras-common                  CentOS Stream 8 - Extras common packages
```

________________________________________________________________________________________________


see list of addresses of the online repositories that your machine uses


```bash
[bob@centos-host ~]$ sudo yum repolist -v

Repo-id            : appstream
Repo-name          : CentOS Stream 8 - AppStream
Repo-revision      : 8-stream
Repo-distro-tags      : [cpe:/o:centos-stream:centos-stream:8]:  ,  , 8, C, O, S, S, a,
                      : e, e, m, n, r, t, t
Repo-updated       : Tue May  9 15:24:38 2023
Repo-pkgs          : 24171
Repo-available-pkgs: 21580
Repo-size          : 59 G
Repo-mirrors       : http://mirrorlist.centos.org/?release=8-stream&arch=x86_64&repo=AppStream&infra=stock
Repo-baseurl       : http://mirror.cogentco.com/pub/linux/centos/8-stream/AppStream/x86_64/os/
                   : (9 more)
Repo-expire        : 172800 second(s) (last: Sun May 21 13:25:50 2023)
Repo-filename      : /etc/yum.repos.d/CentOS-Stream-AppStream.repo
```

________________________________________________________________________________________________


show status of all repos

```bash
[bob@centos-host ~]$ sudo yum repolist --all

repo id                       repo name                                          status
appstream                     CentOS Stream 8 - AppStream                        enabled
appstream-source              CentOS Stream 8 - AppStream - Source               disabled
baseos                        CentOS Stream 8 - BaseOS                           enabled
baseos-source                 CentOS Stream 8 - BaseOS - Source                  disabled
debuginfo                     CentOS Stream 8 - Debuginfo                        disabled
extras                        CentOS Stream 8 - Extras                           enabled
extras-common                 CentOS Stream 8 - Extras common packages           enabled
extras-source                 CentOS Stream 8 - Extras - Source                  disabled
ha                            CentOS Stream 8 - HighAvailability                 disabled
ha-source                     CentOS Stream 8 - HighAvailability - Source        disabled
media-appstream               CentOS Stream 8 - Media - AppStream                disabled
media-baseos                  CentOS Stream 8 - Media - BaseOS                   disabled
nfv                           CentOS Stream 8 - NFV                              disabled
nfv-source                    CentOS Stream 8 - NFV - Source                     disabled
powertools                    CentOS Stream 8 - PowerTools                       disabled
powertools-source             CentOS Stream 8 - PowerTools - Source              disabled
resilientstorage              CentOS Stream 8 - ResilientStorage                 disabled
resilientstorage-source       CentOS Stream 8 - ResilientStorage - Source        disabled
rt                            CentOS Stream 8 - RealTime                         disabled
rt-source                     CentOS Stream 8 - RT - Source                      disabled
```

________________________________________________________________________________________________


to enable/disable a repo by subscription-manager

```bash
[bob@centos-host ~]$ sudo subscription-manager repos --enable appstream-source
```

```bash
[bob@centos-host ~]$ sudo subscription-manager repos --disable appstream-source
```

________________________________________________________________________________________________


to enable/disable a repo by yum

```bash
[bob@centos-host ~]$ sudo yum-config-manager repos --enable appstream-source
```

```bash
[bob@centos-host ~]$ sudo yum-config-manager repos --disable appstream-source
```

________________________________________________________________________________________________


# install yum

```bash
[bob@centos-host ~]$ sudo yum install yum-utils
```

________________________________________________________________________________________________


add repo by yum

```bash
[bob@centos-host ~]$ sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

________________________________________________________________________________________________



```bash
[bob@centos-host ~]$ sudo yum repolist -v | grep filename

Repo-filename      : /etc/yum.repos.d/CentOS-Stream-AppStream.repo
Repo-filename      : /etc/yum.repos.d/CentOS-Stream-BaseOS.repo
Repo-filename      : /etc/yum.repos.d/docker-ce.repo
Repo-filename      : /etc/yum.repos.d/CentOS-Stream-Extras.repo
Repo-filename      : /etc/yum.repos.d/CentOS-Stream-Extras-common.repo
```

________________________________________________________________________________________________


to remove the repo, we can remove the file

```bash
[bob@centos-host ~]$ cat /etc/yum.repos.d/docker-ce.repo

[docker-ce-stable]
name=Docker CE Stable - $basearch
baseurl=https://download.docker.com/linux/centos/$releasever/$basearch/stable
enabled=1
gpgcheck=1
gpgkey=https://download.docker.com/linux/centos/gpg
```

________________________________________________________________________________________________


### search for a package

```bash
[bob@centos-host ~]$ yum search 'web server'
  
============================== Summary Matched: web server ==============================
libcurl.i686 : A library for getting files from web servers
libcurl.x86_64 : A library for getting files from web servers
nginx.x86_64 : A high performance web server and reverse proxy server
pcp-pmda-weblog.x86_64 : Performance Co-Pilot (PCP) metrics from web server logs
```

________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ yum info nginx

Last metadata expiration check: 0:01:26 ago on Sun May 21 14:51:06 2023.
Available Packages
Name         : nginx
Epoch        : 1
Version      : 1.14.1
Release      : 9.module_el8.0.0+1060+3ab382d3
Architecture : x86_64
Size         : 570 k
Source       : nginx-1.14.1-9.module_el8.0.0+1060+3ab382d3.src.rpm
Repository   : appstream
Summary      : A high performance web server and reverse proxy server
URL          : http://nginx.org/
License      : BSD
Description  : Nginx is a web server and a reverse proxy server for HTTP, SMTP, POP3 and
             : IMAP protocols, with a strong focus on high concurrency, performance and
             : low memory usage.
```

________________________________________________________________________________________________


### install package

```bash
[bob@centos-host ~]$ sudo yum install nginx
```

________________________________________________________________________________________________


if we accidently deleted the config files, we can re install the package

```bash
[bob@centos-host ~]$ sudo yum reinstall nginx
```

________________________________________________________________________________________________


remove a package

```bash
[bob@centos-host ~]$ sudo yum remove nginx
```

________________________________________________________________________________________________


### Group: install several software components at once

```bash
[bob@centos-host ~]$ yum group list

Available Environment Groups:
   Server with GUI
   Server
   Minimal Install
   Workstation
   Virtualization Host
   Custom Operating System
Installed Groups:
   Development Tools
Available Groups:
   Container Management
   .NET Core Development
   RPM Development Tools
   Graphical Administration Tools
   Headless Management
   Legacy UNIX Compatibility
   Network Servers
   Scientific Support
   Security Tools
   Smart Card Support
   System Tools
```

________________________________________________________________________________________________


to see all groups include the hidden ones

```bash
[bob@centos-host ~]$ yum group list --hidden
```

________________________________________________________________________________________________


install / remove package group

```bash
[bob@centos-host ~]$ sudo yum group install 'Server with GUI'
```

```bash
[bob@centos-host ~]$ sudo yum group remove 'Server with GUI'
```

________________________________________________________________________________________________


## RPM

```bash
[bob@centos-host ~]$ wget http://download.nomachine.com/download/5.1/Linux/nomachine_5.1.26_1_x86_64.rpm
```



```bash
[bob@centos-host ~]$ sudo yum install ./nomachine_5.1.26_1_x86_64.rpm
```

OR


```bash
[bob@centos-host ~]$ sudo rpm -ivh ./nomachine_5.1.26_1_x86_64.rpm
```


________________________________________________________________________________________________


remove the package 

then

remove the dependencies

```bash
[bob@centos-host ~]$ sudo yum remove nomachine
```


```bash
[bob@centos-host ~]$ sudo yum autoremove
```


________________________________________________________________________________________________


### update

to see which packages can be updated on the system

```bash
[bob@centos-host ~]$ sudo yum check-upgrade
```

________________________________________________________________________________________________


update all packages to the latest version in the repositories

```bash
[bob@centos-host ~]$  sudo yum update
```

________________________________________________________________________________________________
