
Tuned is a daemon in Red Hat Enterprise Linux that dynamically tunes system settings and configurations based on the workload and usage patterns.

It is a part of the Red Hat Performance Tuning Toolkit and is designed to improve system performance, stability, and reliability.

Tuned monitors various system parameters, such as `CPU utilization`, `memory usage`, `I/O load`, and `network traffic`, and adjusts system settings accordingly.

For example, it can adjust the CPU frequency scaling governor, disk I/O scheduler, network packet size, and other parameters to optimize system performance for a given workload.

Tuned comes with a set of `pre-defined` `profiles`, each designed for a specific workload or usage pattern, such as virtualization, database servers, desktop systems, and so on.

Users can also `create` their own `custom` `profiles` and configure Tuned to use them.

________________________________________________________________________________________________


`2` main profiles:

- `power saving` profiles

- `performance boosting` profiles



`default` profile: `balanced` profile 


________________________________________________________________________________________________


```bash
yum install tuned
```

________________________________________________________________________________________________


```bash
systemctl enable --now tuned
```

________________________________________________________________________________________________

## `tuned-adm active`

show active profile

```bash
[bob@centos-host ~]$ tuned-adm active
Current active profile: virtual-guest
```

________________________________________________________________________________________________


## `tuned-adm verify`

checks that the current system settings match the current tuned profile

```bash
[bob@centos-host ~]$ sudo tuned-adm verify

Verification failed, current system settings differ from the preset profile.
You can mostly fix this by restarting the TuneD daemon, e.g.:
  systemctl restart tuned
or
  service tuned restart
Sometimes (if some plugins like bootloader are used) a reboot may be required.
See TuneD log file ('/var/log/tuned/tuned.log') for details.
```

________________________________________________________________________________________________


to see TuneD `profiles` on the system

```bash
ls /usr/lib/tuned
```

________________________________________________________________________________________________



the `configuration`

```bash
[bob@centos-host ~]$ cat /etc/tuned/tuned-main.conf
```

________________________________________________________________________________________________





```bash
[bob@centos-host ~]$ man tuned.conf
```

________________________________________________________________________________________________


## `tuned-adm list`

list the available profiles

```bash
[bob@centos-host ~]$ tuned-adm list

Available profiles:
- accelerator-performance     - Throughput performance based tuning with disabled higher latency STOP states
- aws                         - Optimize for aws ec2 instances
- balanced                    - General non-specialized tuned profile
- desktop                     - Optimize for the desktop use-case
- hpc-compute                 - Optimize for HPC compute workloads
- intel-sst                   - Configure for Intel Speed Select Base Frequency
- latency-performance         - Optimize for deterministic performance at the cost of increased power consumption
- network-latency             - Optimize for deterministic performance at the cost of increased power consumption, focused on low latency network performance
- network-throughput          - Optimize for streaming network throughput, generally only necessary on older CPUs or 40G+ networks
- optimize-serial-console     - Optimize for serial console use.
- powersave                   - Optimize for low power consumption
- throughput-performance      - Broadly applicable tuning that provides excellent performance across a variety of common server workloads
- virtual-guest               - Optimize for running inside a virtual guest
- virtual-host                - Optimize for running KVM guests
Current active profile: virtual-guest
```

________________________________________________________________________________________________




## `tuned-adm profile`


```bash
[bob@centos-host ~]$ tuned-adm profile

Available profiles:
- accelerator-performance     - Throughput performance based tuning with disabled higher latency STOP states
- aws                         - Optimize for aws ec2 instances
- balanced                    - General non-specialized tuned profile
- desktop                     - Optimize for the desktop use-case
- hpc-compute                 - Optimize for HPC compute workloads
- intel-sst                   - Configure for Intel Speed Select Base Frequency
- latency-performance         - Optimize for deterministic performance at the cost of increased power consumption
- network-latency             - Optimize for deterministic performance at the cost of increased power consumption, focused on low latency network performance
- network-throughput          - Optimize for streaming network throughput, generally only necessary on older CPUs or 40G+ networks
- optimize-serial-console     - Optimize for serial console use.
- powersave                   - Optimize for low power consumption
- throughput-performance      - Broadly applicable tuning that provides excellent performance across a variety of common server workloads
- virtual-guest               - Optimize for running inside a virtual guest
- virtual-host                - Optimize for running KVM guests
Current active profile: virtual-guest
```

________________________________________________________________________________________________


## `tuned-adm profile balanced`

switch the profile

```bash
[bob@centos-host ~]$ sudo tuned-adm profile balanced

[bob@centos-host ~]$ sudo tuned-adm active
Current active profile: balanced
```

________________________________________________________________________________________________




## `tuned-adm profile_info`

info of current profile

```bash
[bob@centos-host ~]$ tuned-adm profile_info

Profile name:
balanced

Profile summary:
General non-specialized tuned profile

Profile description:
```

________________________________________________________________________________________________



## `tuned-adm recommend`


see the recommended profile

```bash
[bob@centos-host ~]$ tuned-adm recommend

virtual-guest
```

________________________________________________________________________________________________



## `tuned-adm auto_profile`


enable automatic profile selection

```bash
[bob@centos-host ~]$ tuned-adm auto_profile 
```

________________________________________________________________________________________________





## `tuned-adm profile_mode`


```bash
[bob@centos-host ~]$ tuned-adm profile_mode

Profile selection mode: manual
```

________________________________________________________________________________________________




merge 2 or more profiles

```bash
[bob@centos-host ~]$ sudo tuned-adm profile balanced powersave
```

________________________________________________________________________________________________



Dynamic Tuning


```bash
[bob@centos-host ~]$ grep "dynamic_tuning" /etc/tuned/tuned-main.conf 
dynamic_tuning = 0
```

to enable dynamic tuning, we can change this value and restart the service


________________________________________________________________________________________________


Optimize the system to run in a virtual machine for the best performance and concurrently tunes it for low power consumption. Low power consumption is the priority.

```bash
tuned-adm profile virtual-guest powersave
```

________________________________________________________________________________________________

