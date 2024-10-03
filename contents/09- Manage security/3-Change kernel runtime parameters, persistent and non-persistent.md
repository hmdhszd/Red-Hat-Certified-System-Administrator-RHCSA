




### Understanding Kernel Parameters for RHCSA Exam

**Kernel Parameters** are key settings that configure the behavior of the Linux kernel. The kernel is the core of the operating system, responsible for managing hardware, system processes, and resources like memory and CPU. Modifying kernel parameters allows you to adjust how the system behaves at a low level. This is a crucial skill for system administrators, and it is covered in the **RHCSA exam**.

#### Where Kernel Parameters Are Set:
1. **/proc/sys** directory (Temporary settings)
2. **/etc/sysctl.conf** or specific files in **/etc/sysctl.d/** (Persistent settings)

### Key Kernel Parameters Concepts:

1. **Runtime (Temporary) Kernel Parameters:**
   - You can temporarily change kernel parameters at runtime using the `sysctl` command or by directly interacting with files in the `/proc/sys/` directory.
   - These changes are effective immediately but do **not** persist across reboots.

2. **Persistent Kernel Parameters:**
   - To make kernel parameter changes persist across reboots, you must add them to the `/etc/sysctl.conf` file or create a custom file in `/etc/sysctl.d/`.
   - This ensures that the settings are applied during the boot process.

---

### Practical Kernel Parameter Examples:

Let's look at some common examples that you might encounter in the **RHCSA exam**.

#### 1. **Checking Current Kernel Parameter Values:**

You can view the current value of a kernel parameter by inspecting files in `/proc/sys/`. For example:

```bash
cat /proc/sys/net/ipv4/ip_forward
```

This command checks if IP forwarding is enabled on your system. A result of `0` means it’s disabled, while `1` means it’s enabled.

#### 2. **Temporarily Changing Kernel Parameters Using `sysctl`:**

To enable IP forwarding temporarily, you can use:

```bash
sysctl -w net.ipv4.ip_forward=1
```

This change will not persist after a reboot. To confirm the change, run:

```bash
sysctl net.ipv4.ip_forward
```

#### 3. **Making Kernel Parameters Persistent:**

To ensure that IP forwarding remains enabled after a reboot, you need to modify the `/etc/sysctl.conf` file or create a file in `/etc/sysctl.d/`:

```bash
echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf
```

Then apply the changes:

```bash
sysctl -p
```

This command reloads the `/etc/sysctl.conf` file and applies all the changes in it.

#### 4. **Using `/etc/sysctl.d/` for Custom Configuration Files:**

It’s often cleaner to create custom configuration files for kernel parameters rather than modifying `/etc/sysctl.conf` directly. You can do this by adding a `.conf` file to `/etc/sysctl.d/`.

Example:

```bash
echo "net.ipv4.ip_forward = 1" > /etc/sysctl.d/99-custom.conf
sysctl --system
```

This will load the parameter at boot and apply it immediately.

---

### RHCSA Exam Scenarios for Kernel Parameters:

1. **Enabling IP Forwarding:**
   You might be asked to enable IP forwarding (which allows routing between networks). To solve this, you would:
   - Temporarily enable it using `sysctl -w`.
   - Ensure it's enabled after reboot by editing `/etc/sysctl.conf` or creating a file in `/etc/sysctl.d/`.

2. **Increasing the Number of File Handles:**
   File handles are controlled by the kernel parameter `fs.file-max`. If asked to increase the number of file handles the system can open, you could:
   
   **Check current value:**
   ```bash
   cat /proc/sys/fs/file-max
   ```
   **Temporarily change:**
   ```bash
   sysctl -w fs.file-max=200000
   ```
   **Make persistent:**
   ```bash
   echo "fs.file-max = 200000" >> /etc/sysctl.conf
   sysctl -p
   ```

3. **Optimizing Network Performance:**
   You might need to tune network-related parameters like TCP buffer sizes or connection tracking. This can be done in a similar manner:
   
   **Example (Increase the maximum number of connections):**
   ```bash
   echo "net.netfilter.nf_conntrack_max = 100000" >> /etc/sysctl.d/99-custom.conf
   sysctl --system
   ```

---

### Key Commands to Remember:

1. **sysctl -w**: Temporarily change a kernel parameter.
   ```bash
   sysctl -w kernel.parameter=value
   ```

2. **sysctl -p**: Reload the configuration file `/etc/sysctl.conf`.
   ```bash
   sysctl -p
   ```

3. **sysctl --system**: Load parameters from all configuration files in `/etc/sysctl.conf` and `/etc/sysctl.d/`.
   ```bash
   sysctl --system
   ```

4. **cat /proc/sys/**: Read current kernel parameter values from the `/proc/sys` directory.

---

### RHCSA Exam Tips:
- Focus on scenarios like enabling IP forwarding, adjusting file handle limits, and tuning network settings.
- Be prepared to **make changes persistent** across reboots.
- Practice both temporary and persistent methods using `sysctl`, `/proc/sys/`, and `/etc/sysctl.d/`.

Would you like to try out a hands-on exercise or delve into specific parameters that are frequently tested?









Here are some **realistic sample questions** that you might encounter in the **RHCSA exam** related to **Kernel Parameters**. These focus on topics that are frequently tested in the RHCSA exam, particularly around system tuning and configuration persistence.

---

### **Sample Question 1: Enable IP Forwarding**
**Objective:** Enable IP forwarding temporarily, verify it, and make the change persist across reboots.

**Task:**
1. Temporarily enable IP forwarding.
2. Verify that IP forwarding is enabled.
3. Ensure that IP forwarding remains enabled after a reboot.

**Steps to Answer:**
1. **Temporarily enable IP forwarding:**
   ```bash
   sysctl -w net.ipv4.ip_forward=1
   ```
2. **Verify the change:**
   ```bash
   sysctl net.ipv4.ip_forward
   # OR
   cat /proc/sys/net/ipv4/ip_forward
   ```
   Expected result: `1` (enabled)

3. **Make the change persistent:**
   Add the setting to `/etc/sysctl.conf` or `/etc/sysctl.d/`:
   ```bash
   echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf
   ```
   Apply the changes:
   ```bash
   sysctl -p
   ```

---

### **Sample Question 2: Modify File Handle Limits**
**Objective:** Increase the number of file handles that can be opened on the system.

**Task:**
1. Increase the system-wide maximum number of file handles to 500,000.
2. Ensure this change persists across reboots.

**Steps to Answer:**
1. **Temporarily change the file handle limit:**
   ```bash
   sysctl -w fs.file-max=500000
   ```
2. **Make the change persistent:**
   Add the setting to `/etc/sysctl.conf`:
   ```bash
   echo "fs.file-max = 500000" >> /etc/sysctl.conf
   ```
3. **Apply the changes:**
   ```bash
   sysctl -p
   ```

4. **Verify the new value:**
   ```bash
   cat /proc/sys/fs/file-max
   ```

---

### **Sample Question 3: Configure Connection Tracking Limits**
**Objective:** Modify the maximum number of tracked connections in the kernel.

**Task:**
1. Increase the maximum number of network connections tracked by `conntrack` to 200,000.
2. Make sure this change persists after reboot.

**Steps to Answer:**
1. **Temporarily increase the connection tracking limit:**
   ```bash
   sysctl -w net.netfilter.nf_conntrack_max=200000
   ```
2. **Make the change persistent:**
   You can either add this setting to `/etc/sysctl.conf` or create a custom configuration file in `/etc/sysctl.d/`. For example:
   ```bash
   echo "net.netfilter.nf_conntrack_max = 200000" > /etc/sysctl.d/99-conntrack.conf
   ```
3. **Apply the changes:**
   ```bash
   sysctl --system
   ```

4. **Verify the new value:**
   ```bash
   cat /proc/sys/net/netfilter/nf_conntrack_max
   ```

---

### **Sample Question 4: Set Kernel SHMMAX Value**
**Objective:** Modify the shared memory limits of the system.

**Task:**
1. Set the shared memory maximum (`shmmax`) to 1 GB.
2. Ensure the change persists across reboots.

**Steps to Answer:**
1. **Convert 1 GB to bytes:**
   - 1 GB = 1,073,741,824 bytes (use a calculator if necessary).
   
2. **Temporarily change the `shmmax` value:**
   ```bash
   sysctl -w kernel.shmmax=1073741824
   ```
3. **Make the change persistent:**
   Add the setting to `/etc/sysctl.conf`:
   ```bash
   echo "kernel.shmmax = 1073741824" >> /etc/sysctl.conf
   ```
4. **Apply the changes:**
   ```bash
   sysctl -p
   ```

5. **Verify the new value:**
   ```bash
   cat /proc/sys/kernel/shmmax
   ```

---

### **Sample Question 5: Set Kernel Panic Timeout**
**Objective:** Set a timeout for the system to automatically reboot after a kernel panic.

**Task:**
1. Set the kernel to automatically reboot 10 seconds after a panic.
2. Ensure the change is persistent across reboots.

**Steps to Answer:**
1. **Temporarily set the kernel panic timeout:**
   ```bash
   sysctl -w kernel.panic=10
   ```

2. **Make the change persistent:**
   Add the setting to `/etc/sysctl.conf`:
   ```bash
   echo "kernel.panic = 10" >> /etc/sysctl.conf
   ```

3. **Apply the changes:**
   ```bash
   sysctl -p
   ```

4. **Verify the change:**
   ```bash
   cat /proc/sys/kernel/panic
   ```

---

### **Sample Question 6: Optimize TCP Memory Settings**
**Objective:** Adjust TCP memory allocation to improve network performance.

**Task:**
1. Set the minimum, default, and maximum TCP memory settings for a network buffer as follows: 
   - Minimum: 4 MB
   - Default: 16 MB
   - Maximum: 64 MB
2. Ensure the changes are persistent across reboots.

**Steps to Answer:**
1. **Convert MB values to bytes:**
   - 4 MB = 4194304 bytes
   - 16 MB = 16777216 bytes
   - 64 MB = 67108864 bytes

2. **Temporarily change the TCP memory settings:**
   ```bash
   sysctl -w net.ipv4.tcp_mem="4194304 16777216 67108864"
   ```

3. **Make the change persistent:**
   Add the setting to `/etc/sysctl.conf` or create a custom configuration file:
   ```bash
   echo "net.ipv4.tcp_mem = 4194304 16777216 67108864" > /etc/sysctl.d/99-tcp.conf
   ```

4. **Apply the changes:**
   ```bash
   sysctl --system
   ```

5. **Verify the change:**
   ```bash
   sysctl net.ipv4.tcp_mem
   ```

---

### **Tips for the Exam:**
- **Understand the `/proc/sys/` hierarchy:** Familiarize yourself with how to read kernel parameter values directly from the `/proc/sys/` virtual file system.
- **Focus on persistence:** Kernel parameters can be changed temporarily or permanently; make sure you practice both methods.
- **Common kernel parameters:** Parameters like `ip_forward`, `file-max`, `shmmax`, and `nf_conntrack_max` are frequently tested.
- **Use `sysctl -w` for temporary changes** and make sure to persist settings using `/etc/sysctl.conf` or `/etc/sysctl.d/`.

Would you like to try configuring one of these parameters in a hands-on practice setup?
























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


`0`   -->   `Disable`

`1`   -->   `Enable`



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


