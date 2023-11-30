
## RedHat 8

when system is booting up, press "e" to edit default kernel lines


________________________________________________________________________________________________



password recovery:

add "re.break" after "quiet"

then press CTRL + X

________________________________________________________________________________________________



after that you'll see the break point or emergency mode shell


```bash
mount -o remount,rw /sysroot
```


```bash
chroot /sysroot
```

________________________________________________________________________________________________



now we can change root password

```bash
passwd root
```

________________________________________________________________________________________________



set a flag so that SELinux relabels the system as needed on the next boot

```bash
touch /.autorelabel
```

________________________________________________________________________________________________




```bash
exit

reboot
```

________________________________________________________________________________________________



## RedHat 9


when system is booting up, press "e" to edit default kernel lines


________________________________________________________________________________________________



password recovery:

add "init=/bin/bash" after "quiet"

then press CTRL + X

________________________________________________________________________________________________




now we can change root password

```bash
passwd root
```

________________________________________________________________________________________________



set a flag so that SELinux relabels the system as needed on the next boot

```bash
touch /.autorelabel
```

________________________________________________________________________________________________

then reboot the system:


```bash
exec /sbin/init
```

________________________________________________________________________________________________

