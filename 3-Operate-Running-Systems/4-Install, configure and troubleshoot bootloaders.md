

we select "troubleshooting" instead of normal CentOS Install

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



On `BIOS-based` machines, issue the following command as root:

```bash
grub2-mkconfig -o /boot/grub2/grub.cfg
```

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


install the grub on the disk that has the boot partition:

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


________________________________________________________________________________________________



see the grub settings:

```bash
/etc/default/grub
```

________________________________________________________________________________________________



change the timeout of the grub:

```bash
GRUB_TIMEOUT
```

________________________________________________________________________________________________


after changing this file, we should regenerate the config file:

```bash
grub2-mkconfig -o /boot/grub2/grub.cfg
```

________________________________________________________________________________________________
