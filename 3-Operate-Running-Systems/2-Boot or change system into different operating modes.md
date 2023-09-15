 
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

1 - `Single` user mode

2 - `Multi`-user mode `without` `networking` (`without` `GUI`)

3 - `Multi`-user mode with `networking` (`without` `GUI`)

4 - Not used/User-definable

5 - `Multi`-user `Graphical` user interface (with GUI) mode with `networking`

6 - `Reboot` state


________________________________________________________________________________________________

 ## `systemctl poweroff`

0 - Power-off state:


```bash
sudo systemctl poweroff
```

________________________________________________________________________________________________

 ## `systemctl rescue`

1 - Single user mode


```bash
sudo systemctl rescue
```

in rescue target, more programms will be loaded than the emergency target

________________________________________________________________________________________________

 ## `systemctl isolate multi-user.target`

2 - Multi-user mode without networking


```bash
sudo systemctl isolate multi-user.target
```

________________________________________________________________________________________________

 ## `systemctl isolate ??????`

3 - Multi-user mode with networking ??????????????


```bash
sudo systemctl isolate graphical.target
```

________________________________________________________________________________________________


4 - Not used/User-definable:

The fourth target is not defined by default. It is available for users to define their own targets.



________________________________________________________________________________________________

 ## `systemctl isolate graphical.target`

5 - Graphical user interface (GUI) mode with networking


```bash
sudo systemctl isolate graphical.target
```

________________________________________________________________________________________________


 ## `systemctl reboot`

6 - Reboot state

```bash
sudo systemctl reboot
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
