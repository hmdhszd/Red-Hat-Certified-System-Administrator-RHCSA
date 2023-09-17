
# sshd config on the `server`


## `/etc/ssh/sshd_config`


```bash
[bob@centos-host ~]$ sudo cat /etc/ssh/sshd_config 
```

________________________________________________________________________________________________


the port number that the sshd expect ssh connections

```bash
#Port 22
```

________________________________________________________________________________________________


## `AddressFamily`


ipv4 or ipv6 or any


for `ipv4`      -->      `inet`

for `ipv6`      -->      `inet6`

for `both`      -->      `any`


```bash
#AddressFamily any
```

________________________________________________________________________________________________


## `ListenAddress`


who can ssh to this server (default 0.0.0.0 = any)

```bash
#ListenAddress 0.0.0.0
```

________________________________________________________________________________________________


## `PermitRootLogin`


enable / disable login with root user

```bash
PermitRootLogin yes
```

________________________________________________________________________________________________


## `PasswordAuthentication`


login with `password` or only `ssh key`

```bash
PasswordAuthentication yes
```

________________________________________________________________________________________________


we can disable PasswordAuthentication for all users and enable it only for one user

```bash
PasswordAuthentication no

Match User Hamid
      PasswordAuthentication yes
```

________________________________________________________________________________________________


## `X11Forwarding`

`X11 forwarding` is a feature in the SSH (Secure Shell) protocol that allows graphical applications running on a remote server to be displayed on a local machine.

It enables users to run `GUI-based` `applications` on a `remote server` and have their graphical output redirected to their local machine.

```bash
X11Forwarding yes
```

________________________________________________________________________________________________


## `systemctl reload sshd.service`

after changing the configuration, we should reload the service

```bash
[bob@centos-host ~]$ sudo systemctl reload sshd.service
```

________________________________________________________________________________________________


# ssh client config on the `client`

## `ssh-keygen`

create ssh key

```bash
[bob@centos-host ~]$ ssh-keygen

Generating public/private rsa key pair.
Enter file in which to save the key (/home/bob/.ssh/id_rsa): 
Created directory '/home/bob/.ssh'.
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/bob/.ssh/id_rsa.
Your public key has been saved in /home/bob/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:gTrr15nMvHsL/sgGB5K4e+V8z1syLBf4DmxAjiNtC9Y bob@centos-host
The key's randomart image is:
+---[RSA 3072]----+
|                 |
|       .         |
|   . .o .        |
|  .oo=.  o       |
|  +.E.o.S .      |
| ..+ =oo.o .     |
|   .o+ B*o* .    |
|  ... ++XO.+     |
|   ... o**Bo     |
+----[SHA256]-----+
```

________________________________________________________________________________________________



## `/home/bob/.ssh/`

`Public` and `Private` keys

```bash
[bob@centos-host ~]$ ls /home/bob/.ssh/

id_rsa  id_rsa.pub
```

________________________________________________________________________________________________


## `.ssh/config`

### ssh `client` config file should be `created` `manually`

```bash
[bob@centos-host ~]$ sudo vi .ssh/config

Host my-server1
        HostName 1.2.3.4
        Port 22
        User hamid
```



```bash
[bob@centos-host ~]$ sudo chmod 600 .ssh/config
```


```bash
[bob@centos-host ~]$ ssh my-server1
```

________________________________________________________________________________________________

## `ssh-copy-id` --> `~/.ssh/authorized_keys` (on the server side)


#### in this case, the public key of the client will be copied to the `server` (~/.ssh/authorized_keys)

```bash
[bob@centos-host ~]$ ssh-copy-id hamid@127.0.0.1

/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/bob/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
hamid@127.0.0.1's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'hamid@127.0.0.1'"
and check to make sure that only the key(s) you wanted were added.
```

________________________________________________________________________________________________



Also you can do it manually!

you can log into the `server` and copy the public key into that path

this file should have `600` `permission`

```bash
chmod 600 ~/.ssh/authorized_keys
```

________________________________________________________________________________________________


## '.ssh/known_hosts' (on the Client side)

when we ssh to a server for the first time, the fingerprint of the server will save at this file on the ssh client

to remove it use -R

```bash
[bob@centos-host ~]$ cat .ssh/known_hosts

127.0.0.1 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBFU81eko9npTtVm3ZEoCZrKAoL7WvcTb1L9BWOc5EdaX+lMoJGkQPMX7SRja7kOymQe4ZBJGqdSg75SzpJtble4=
```



## `ssh-keygen -R`

```bash
[bob@centos-host ~]$ ssh-keygen -R 127.0.0.1

# Host 127.0.0.1 found: line 1
/home/bob/.ssh/known_hosts updated.
Original contents retained as /home/bob/.ssh/known_hosts.old
```

________________________________________________________________________________________________

# `Default` configurations of SSH `Client`

## `/etc/ssh/ssh_config`


```bash
[bob@centos-host ~]$ cat /etc/ssh/ssh_config
```

but it's not recommended to change it, because it will be overwriten when it's overwrite

instead

you can create your own ssh config file


```bash
[bob@centos-host ~]$ sudo vi /etc/ssh/ssh_config.d/99-my-ssh-settings.conf

port 222 
```

________________________________________________________________________________________________
