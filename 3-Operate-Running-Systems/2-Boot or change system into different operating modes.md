# Boot Target in Linux

________________________________________________________________________________________________

 ## `systemctl get-default`

to see default boot target

```bash
[bob@centos-host ~]$ systemctl get-default
multi-user.target
```

________________________________________________________________________________________________

 ## `systemctl set-default`

change default boot target

(will be applied after rebooting)

```bash
[bob@centos-host ~]$ sudo systemctl set-default graphical.target
Removed /etc/systemd/system/default.target.
Created symlink /etc/systemd/system/default.target → /usr/lib/systemd/system/graphical.target.
```

________________________________________________________________________________________________

 ## `systemctl isolate`

switch target without rebooting

```bash
[bob@centos-host ~]$ sudo systemctl isolate graphical.target 
```

________________________________________________________________________________________________


## boot targets in linux:



0 - `Power-off` state

1 - `Single` user mode `without` `networking` (`without` `GUI`)

2 - `Multi`-user mode `without` `networking` (`without` `GUI`)

3 - `Multi`-user mode `with` `networking`, (`without` `GUI`)

4 - Not used/User-definable

5 - `Multi`-user mode `with` `networking`, (`with` `GUI`)

6 - `Reboot` state


________________________________________________________________________________________________

 ## `systemctl poweroff`

## 0 - `poweroff.target` / `runlevel0.target`:


```bash
sudo systemctl poweroff
```

OR

```bash
sudo systemctl isolate runlevel0.target
```


Change the default boot target to `poweroff.target` :

```bash
[bob@centos-host ~]$ sudo systemctl set-default runlevel0.target

Removed /etc/systemd/system/default.target.
Created symlink /etc/systemd/system/default.target → /usr/lib/systemd/system/poweroff.target.
```

```bash
[bob@centos-host ~]$ sudo systemctl get-default 
poweroff.target
```
________________________________________________________________________________________________

 ## `systemctl rescue`

## 1 - `rescue.target` / `runlevel1.target`: Single user mode


in rescue target, more programms will be loaded than the emergency target

```bash
sudo systemctl rescue
```

OR

```bash
sudo systemctl isolate runlevel1.target
```


Change the default boot target to `rescue.target` :



```bash
[bob@centos-host ~]$ sudo systemctl set-default runlevel1.target

Removed /etc/systemd/system/default.target.
Created symlink /etc/systemd/system/default.target → /usr/lib/systemd/system/rescue.target.
```


```bash
[bob@centos-host ~]$ sudo systemctl get-default 
rescue.target
```

________________________________________________________________________________________________

 ## `systemctl isolate multi-user.target`

## 2 - `multi-user.target` / `runlevel2.target`: Multi-user mode without networking



```bash
sudo systemctl isolate multi-user.target
```



Change the default boot target to `multi-user.target` :


```bash
[bob@centos-host ~]$ sudo systemctl set-default runlevel2.target

Removed /etc/systemd/system/default.target.
Created symlink /etc/systemd/system/default.target → /usr/lib/systemd/system/multi-user.target.
```

```bash
[bob@centos-host ~]$ sudo systemctl get-default 
multi-user.target
```

________________________________________________________________________________________________

 ## `systemctl isolate multi-user.target`

## 3 - `multi-user.target` / `runlevel3.target`: Multi-user mode with networking


```bash
sudo systemctl isolate multi-user.target
```

Change the default boot target to `multi-user.target` :


```bash
[bob@centos-host ~]$ sudo systemctl set-default runlevel3.target

Removed /etc/systemd/system/default.target.
Created symlink /etc/systemd/system/default.target → /usr/lib/systemd/system/multi-user.target.
```

```bash
[bob@centos-host ~]$ sudo systemctl get-default 
multi-user.target
```

________________________________________________________________________________________________


## 4 - Not used/User-definable:

The fourth target is not defined by default. It is available for users to define their own targets.



________________________________________________________________________________________________

 ## `systemctl isolate graphical.target`

## 5 - `graphical.target` / `runlevel5.target`: Graphical user interface (GUI) mode with networking


```bash
sudo systemctl isolate graphical.target
```

Change the default boot target to `graphical.target` :

```bash
[bob@centos-host ~]$ sudo systemctl set-default runlevel5.target

Removed /etc/systemd/system/default.target.
Created symlink /etc/systemd/system/default.target → /usr/lib/systemd/system/graphical.target.
```


```bash
[bob@centos-host ~]$ sudo systemctl get-default 
graphical.target
```

________________________________________________________________________________________________


 ## `systemctl reboot`

## 6 - `reboot.target` / `runlevel6.target`:


```bash
sudo systemctl reboot
```

OR


```bash
sudo systemctl isolate runlevel6.target
```
________________________________________________________________________________________________

## `systemctl isolate emergency.target`

The `emergency` `target` is a special boot target in Linux that is used for `troubleshooting` and `system recovery`.

It is `similar` to `runlevel 1` (`single-user` mode) but provides a `minimal environment` with only `essential services` running. 

The emergency target is usually used when the system cannot boot into any other target, and the user needs to troubleshoot and fix the system manually.

To switch to the emergency target, you can use the following command:

```bash
[bob@centos-host ~]$ sudo systemctl isolate emergency.target
```

________________________________________________________________________________________________


## `systemctl list-units --type target`

```bash
[bob@centos-host ~]$ systemctl list-units --type target

UNIT                   LOAD   ACTIVE SUB    DESCRIPTION                
basic.target           loaded active active Basic System               
cryptsetup.target      loaded active active Local Encrypted Volumes    
getty.target           loaded active active Login Prompts              
local-fs-pre.target    loaded active active Local File Systems (Pre)   
local-fs.target        loaded active active Local File Systems         
multi-user.target      loaded active active Multi-User System          
network-online.target  loaded active active Network is Online          
network.target         loaded active active Network                    
nfs-client.target      loaded active active NFS client services        
nss-user-lookup.target loaded active active User and Group Name Lookups
paths.target           loaded active active Paths                      
remote-fs-pre.target   loaded active active Remote File Systems (Pre)  
remote-fs.target       loaded active active Remote File Systems        
rpc_pipefs.target      loaded active active rpc_pipefs.target          
rpcbind.target         loaded active active RPC Port Mapper            
slices.target          loaded active active Slices                     
sockets.target         loaded active active Sockets                    
sshd-keygen.target     loaded active active sshd-keygen.target         
swap.target            loaded active active Swap                       
sysinit.target         loaded active active System Initialization      
timers.target          loaded active active Timers                     
```




________________________________________________________________________________________________
