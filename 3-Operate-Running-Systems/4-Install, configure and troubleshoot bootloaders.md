## Install Grub bootloader

in order to go to the boot menu, you can press F8 OR keep shift, during the booting process.

we can press `e` to edit the grub settings and press Ctrl+X to reboot with the new settings.

this setting is temporary, if we wanna make it permanent, we can change the `/etc/default/grub`

________________________________________________________________________________________________

OR we select "troubleshooting" instead of normal CentOS Install

________________________________________________________________________________________________


in the boot menu, select the "Rescue a CentOS Stream system"


________________________________________________________________________________________________


then select "1) Continue"


________________________________________________________________________________________________



Press ENTER to get a shell:


________________________________________________________________________________________________


```bash
chroot /mnt/sysroot
```

________________________________________________________________________________________________

`/boot/grub2/grub.cfg`

On `BIOS-based` machines, issue the following command as root:

```bash
grub2-mkconfig -o /boot/grub2/grub.cfg
```

`/boot/efi/EFI/redhat/grub.cfg`

On `UEFI-based` machines, issue the following command as root:

```bash
grub2-mkconfig -o /boot/efi/EFI/redhat/grub.cfg
```



________________________________________________________________________________________________


we will search for the partition that has the `/boot`:

in our case it's sda1

```bash
lsblk
```

________________________________________________________________________________________________

## `grub2-install /dev/sda`

`install` the grub on the disk that has the boot partition:

```bash
grub2-install /dev/sda
```

________________________________________________________________________________________________


exit and reboot

```bash
exit
exit
```

________________________________________________________________________________________________


## change the Grub settings

## `/etc/default/grub`


```bash
[bob@centos-host ~]$ cat /etc/default/grub

GRUB_TIMEOUT=1
GRUB_DISTRIBUTOR="$(sed 's, release .*$,,g' /etc/system-release)"
GRUB_DEFAULT=saved
GRUB_DISABLE_SUBMENU=true
GRUB_TERMINAL_OUTPUT="console"
GRUB_CMDLINE_LINUX="no_timer_check console=tty0 console=ttyS0,115200n8 net.ifnames=0 biosdevname=0 elevator=noop"
GRUB_DISABLE_RECOVERY="true"
GRUB_ENABLE_BLSCFG=true
```

________________________________________________________________________________________________




change the timeout of the grub:

```bash
GRUB_TIMEOUT=1
```

________________________________________________________________________________________________


after changing this file, we should regenerate the config file:

`BIOS-based`:

```bash
grub2-mkconfig -o /boot/grub2/grub.cfg
```

`UEFI-based`:

```bash
grub2-mkconfig -o /boot/efi/EFI/redhat/grub.cfg
```


________________________________________________________________________________________________

## `grubby --info ALL`

see current Grub settings:

```bash
[bob@centos-host ~]$ sudo dnf install grubby
```

```bash
[bob@centos-host ~]$ sudo grubby --info ALL
index=0
kernel="/boot/vmlinuz-4.18.0-240.1.1.el8_3.x86_64"
args="ro no_timer_check console=tty0 console=ttyS0,115200n8 net.ifnames=0 biosdevname=0 elevator=noop $tuned_params"
root="UUID=a62c5b49-755e-41b0-9d36-de3d95e17232"
initrd="/boot/initramfs-4.18.0-240.1.1.el8_3.x86_64.img $tuned_initrd"
title="CentOS Linux (4.18.0-240.1.1.el8_3.x86_64) 8"
id="99023f8aa5784c9c99a1c16aebb7ffa6-4.18.0-240.1.1.el8_3.x86_64"
```

________________________________________________________________________________________________


### change the default kernel:


```bash
[bob@centos-host ~]$ sudo grubby --set-default=/boot/vmlinuz-4.18.0-240.1.1.el8_3.x86_64
```

________________________________________________________________________________________________

### change the default index of the kernel in the boot menu:

```bash
[bob@centos-host ~]$ sudo grubby --set-default-index=1
```

________________________________________________________________________________________________


```bash
[bob@centos-host ~]$ man grubby
```

________________________________________________________________________________________________


Possible boot parameters that you can pass into the linux kernel:

```bash
[bob@centos-host ~]$ man bootparam
```

```bash
[bob@centos-host ~]$ man dracut.cmdline
```


________________________________________________________________________________________________



### On ServerA, ensure that boot messages are displayed, not silenced, for troubleshooting purposes.




- 1: Modify GRUB Configuration File `/etc/default/grub`



- 2: Locate the "GRUB_CMDLINE_LINUX" line and remove `rhgb quiet`.



- 3: Generate a new GRUB configuration file based on the current system configuration:

```bash
grub2-mkconfig -o /boot/grub2/grub.cfg
```


- 4: Restart the system for the changes to take effect:

```bash
reboot
```



________________________________________________________________________________________________



### Create a backup of the Master Boot Record (MBR) located on the device "/dev/sda" of ServerA. Store the backup in the file "/backup/mbr.img":

- Block Size: 512 bytes

- Number of Blocks Copied: 1

- Verification: Confirm successful backup creation




```bash
mkdir -p /backup

dd if=/dev/sda of=/backup/mbr.img bs=512 count=1 status=progress
```


________________________________________________________________________________________________








