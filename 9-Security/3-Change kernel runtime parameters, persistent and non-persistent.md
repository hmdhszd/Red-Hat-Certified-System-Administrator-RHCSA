


### to see all kernel parameters in use:


```bash
[bob@centos-host ~]$ sudo sysctl -a | head

sysctl: permission denied on key 'fs.protected_fifos'
sysctl: permission denied on key 'fs.protected_hardlinks'
sysctl: permission denied on key 'fs.protected_regular'
sysctl: permission denied on key 'fs.protected_symlinks'
sysctl: permission denied on key 'kernel.cad_pid'
sysctl: permission denied on key 'kernel.usermodehelper.bset'
sysctl: permission denied on key 'kernel.usermodehelper.inheritable'
sysctl: permission denied on key 'net.core.bpf_jit_harden'
sysctl: permission denied on key 'net.core.bpf_jit_kallsyms'
sysctl: permission denied on key 'net.core.bpf_jit_limit'
abi.vsyscall32 = 1
crypto.fips_enabled = 0
debug.exception-trace = 1
debug.kprobes-optimization = 1
dev.hpet.max-user-freq = 64
dev.mac_hid.mouse_button2_keycode = 97
dev.mac_hid.mouse_button3_keycode = 100
dev.mac_hid.mouse_button_emulation = 0
.
.
.
```

________________________________________________________________________________________________


every line that starts with "net." is related to network


every line that starts with "vm." is related to virtual memory


every line that starts with "fs." is related to file system


`1`   -->   `Disable`

`0`   -->   `Enable`



________________________________________________________________________________________________


### see the current kernel parameter

```bash
[bob@centos-host ~]$ sudo sysctl net.ipv6.conf.default.disable_ipv6

net.ipv6.conf.default.disable_ipv6 = 0
```

________________________________________________________________________________________________


### change a kernel parameter (Temporary)

```bash
[bob@centos-host ~]$ sudo sysctl -w net.ipv6.conf.default.disable_ipv6=1

net.ipv6.conf.default.disable_ipv6 = 1
```

________________________________________________________________________________________________ 


### change a kernel parameter (Permanently)

to make the changes permanently, we should create a file:

```bash
[bob@centos-host ~]$ vi /etc/sysctl.d/99-sysctl.conf
```

________________________________________________________________________________________________



change swappiness

```bash
[bob@centos-host ~]$ sudo sysctl -a | grep swap

vm.swappiness = 30
```


then add this "vm.swappiness = 30" to this file: /etc/sysctl.d/*.conf


this change will be applied after the first reboot



________________________________________________________________________________________________



to apply this change before reboot:

```bash
[bob@centos-host ~]$ sudo sysctl -p /etc/sysctl.d/99-sysctl.conf

net.ipv4.ip_forward = 1
vm.swappiness = 30
```

________________________________________________________________________________________________



to enable IPv4 and IPv6 packet forwarding:


```bash
echo net.ipv6.conf.all.forwarding=1 > /etc/sysctl.conf

echo net.ipv4.ip_forward=1 > /etc/sysctl.conf


sysctl -p
```




________________________________________________________________________________________________


