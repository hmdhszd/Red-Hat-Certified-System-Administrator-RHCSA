

to see default boot target

```bash
[bob@centos-host ~]$ systemctl get-default
multi-user.target
```

________________________________________________________________________________________________


change default boot target

(will be applied after rebooting)

```bash
[bob@centos-host ~]$ sudo systemctl set-default graphical.target
Removed /etc/systemd/system/default.target.
Created symlink /etc/systemd/system/default.target → /usr/lib/systemd/system/graphical.target.
```

________________________________________________________________________________________________


switch target without rebooting

```bash
[bob@centos-host ~]$ sudo systemctl isolate graphical.target 
```

________________________________________________________________________________________________


## boot targets in linux:



0 - Power-off state

1 - Single user mode

2 - Multi-user mode with no networking

3 - Multi-user mode with networking

4 - Not used/User-definable

5 - Graphical user interface (GUI) mode with networking

6 - Reboot state


________________________________________________________________________________________________


0 - Power-off state:


```bash
sudo systemctl poweroff
```

________________________________________________________________________________________________


1 - Single user mode


```bash
sudo systemctl rescue
```

in rescue target, more programms will be loaded than the emergency target

________________________________________________________________________________________________


2 - Multi-user mode with no networking


```bash
sudo systemctl isolate multi-user.target
```

________________________________________________________________________________________________


3 - Multi-user mode with networking


```bash
sudo systemctl isolate graphical.target
```

________________________________________________________________________________________________


4 - Not used/User-definable:

The fourth target is not defined by default. It is available for users to define their own targets.



________________________________________________________________________________________________


5 - Graphical user interface (GUI) mode with networking


```bash
sudo systemctl isolate graphical.target
```

________________________________________________________________________________________________



6 - Reboot state

```bash
sudo systemctl reboot
```

________________________________________________________________________________________________


The emergency target is a special boot target in Linux that is used for troubleshooting and system recovery.

It is similar to runlevel 1 (single-user mode) but provides a minimal environment with only essential services running. 

The emergency target is usually used when the system cannot boot into any other target, and the user needs to troubleshoot and fix the system manually.

To switch to the emergency target, you can use the following command:

```bash
[bob@centos-host ~]$ sudo systemctl isolate emergency.target
```

________________________________________________________________________________________________
