
## `ps`


```bash
[bob@centos-host ~]$ ps

    PID TTY          TIME CMD
   1589 pts/0    00:00:00 bash
   1613 pts/0    00:00:00 ps
```

________________________________________________________________________________________________


## `top`


```bash
[bob@centos-host ~]$ top

top - 13:32:00 up  1:00,  0 users,  load average: 7.91, 7.91, 7.39
Tasks:  15 total,   1 running,  14 sleeping,   0 stopped,   0 zombie
%Cpu(s):  7.8 us,  7.8 sy,  0.0 ni, 83.5 id,  0.0 wa,  0.0 hi,  0.9 si,  0.0 st
MiB Mem : 209557.7 total,  35437.1 free,  29554.2 used, 144566.4 buff/cache
MiB Swap:      0.0 total,      0.0 free,      0.0 used. 179083.8 avail Mem 

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND          
      1 root      20   0   90348  10580   8268 S   0.0   0.0   0:00.54 systemd          
    320 root      20   0   82168   7368   6480 S   0.0   0.0   0:00.08 systemd-journal  
    471 root      20   0   97608   8236   7196 S   0.0   0.0   0:00.04 systemd-udevd    
    688 rpc       20   0   67240   5736   5012 S   0.0   0.0   0:00.02 rpcbind          
    802 dbus      20   0   54128   5368   4760 S   0.0   0.0   0:00.02 dbus-daemon      
    804 root      20   0   79172   6324   5576 S   0.0   0.0   0:00.03 systemd-machine  
    805 root      20   0    1352    880    780 S   0.0   0.0   0:00.02 ttyd             
    819 root      20   0  109348   3908   3244 S   0.0   0.0   0:00.00 gssproxy         
    928 root      20   0   76640   7708   6776 S   0.0   0.0   0:00.00 sshd             
   1184 dnsmasq   20   0   57644   2776   2316 S   0.0   0.0   0:00.00 dnsmasq          
   1185 root      20   0   57540    444      0 S   0.0   0.0   0:00.00 dnsmasq          
   1575 root      20   0  115636   7480   6504 S   0.0   0.0   0:00.01 sudo             
   1582 root      20   0  109124   5864   4992 S   0.0   0.0   0:00.00 su               
   1589 bob       20   0   19244   3792   3244 S   0.0   0.0   0:00.01 bash             
   1696 bob       20   0   58456   4384   3744 R   0.0   0.0   0:00.00 top
```

________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ ps -aux

USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.0  90348 10580 ?        Ss   12:31   0:00 /sbin/init --log-level
root         320  0.0  0.0  82168  7368 ?        Ss   12:31   0:00 /usr/lib/systemd/syste
root         471  0.0  0.0  97608  8236 ?        Ss   12:31   0:00 /usr/lib/systemd/syste
rpc          688  0.0  0.0  67240  5736 ?        Ss   12:31   0:00 /usr/bin/rpcbind -w -f
dbus         802  0.0  0.0  54128  5368 ?        Ss   12:31   0:00 /usr/bin/dbus-daemon -
root         804  0.0  0.0  79172  6324 ?        Ss   12:31   0:00 /usr/lib/systemd/syste
root         805  0.0  0.0   1420   880 ?        Rs   12:31   0:00 /usr/bin/ttyd -p 8080 
root         819  0.0  0.0 109348  3908 ?        Ssl  12:31   0:00 /usr/sbin/gssproxy -D
root         928  0.0  0.0  76640  7708 ?        Ss   12:31   0:00 /usr/sbin/sshd -D -oCi
dnsmasq     1184  0.0  0.0  57644  2776 ?        S    12:31   0:00 /usr/sbin/dnsmasq --co
root        1185  0.0  0.0  57540   444 ?        S    12:31   0:00 /usr/sbin/dnsmasq --co
root        1575  0.0  0.0 115636  7480 pts/0    Ss   13:27   0:00 sudo su - bob
root        1582  0.0  0.0 109124  5864 pts/0    S    13:27   0:00 su - bob
bob         1589  0.0  0.0  19244  3792 pts/0    S    13:27   0:00 -bash
bob         1644  0.0  0.0  54824  3912 pts/0    R+   13:31   0:00 ps -aux
```

________________________________________________________________________________________________

## `ps <Process ID>`


```bash
[bob@centos-host ~]$ ps 1

    PID TTY      STAT   TIME COMMAND
      1 ?        Ss     0:00 /sbin/init --log-level=err
```


## `ps u <User ID>`

```bash
[bob@centos-host ~]$ ps u 1

USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.0  90348 10580 ?        Ss   12:31   0:00 /sbin/init --log-level
```

________________________________________________________________________________________________

## `ps u -U <User Name>`

user orientation format

```bash
[bob@centos-host ~]$ ps u -U bob

USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
bob         1580  0.0  0.0  19368  3824 pts/0    S    13:44   0:00 -bash
bob         1604  0.0  0.0  51892  3780 pts/0    R+   13:45   0:00 ps u -U bob
```

________________________________________________________________________________________________


## `pgrep -a`


#### search for a process with a specific word in it's `name` ex: proxy

```bash
[bob@centos-host ~]$ pgrep -a proxy

831 /usr/sbin/gssproxy -D
```

________________________________________________________________________________________________


## process niceness

from -20 to 19

lower   -->   less nice   -->   higher priority

________________________________________________________________________________________________

## `nice -n`

change nice value for bash

```bash
[bob@centos-host ~]$ nice -n 11 bash
```

________________________________________________________________________________________________

## `ps -l`

use `-l` to see nice value

```bash
[bob@centos-host ~]$ ps -l  | grep bash

4 S  1000    1580    1573  0  80   0 -  4842 do_wai pts/0    00:00:00 bash
4 S  1000    1684    1580  0  91  11 -  4811 do_wai pts/0    00:00:00 bash
```

________________________________________________________________________________________________


```bash
ps alx
```

________________________________________________________________________________________________


`f`   -->   forest (show `child` `processes`)

```bash
ps fax
```

________________________________________________________________________________________________



```bash
ps -faux
```

________________________________________________________________________________________________

## `renice`

when there is an existing process and you wanna change it's niceness (`process ID`)

```bash
renice 7 1184
```

________________________________________________________________________________________________


## Signals

applications will respond to them if they are programmed to!

SIGHUP - Hangup signal, sent to a process when its controlling terminal is closed.

SIGINT - Interrupt signal, sent to a process when it receives a keyboard interrupt (e.g., Ctrl-C).

SIGQUIT - Quit signal, sent to a process when it receives a keyboard quit command (e.g., Ctrl-).

SIGKILL - Kill signal, used to terminate a process immediately and unconditionally.

SIGTERM - Termination signal, used to ask a process to terminate gracefully.

SIGSTOP - Stop signal, used to pause a process temporarily.

SIGCONT - Continue signal, used to resume a paused process.

SIGUSR1 - User-defined signal 1.

SIGUSR2 - User-defined signal 2.

SIGALRM - Alarm clock signal, used to set a timer that will send a signal to a process when it expires.

SIGCHLD - Child signal, sent to the parent process when a child process terminates.

SIGPIPE - Pipe signal, sent to a process when it tries to write to a pipe that has no readers.

SIGWINCH - Window change signal, sent to a process when its terminal window is resized.

SIGTSTP - Terminal stop signal, sent to a process when it receives a keyboard stop command (e.g., Ctrl-Z).

SIGTTIN - Terminal input signal, sent to a background process that tries to read from its controlling terminal.

SIGTTOU - Terminal output signal, sent to a background process that tries to write to its controlling terminal.


________________________________________________________________________________________________

## `signal -L`

```bash
[bob@centos-host ~]$ signal -L

1) SIGHUP       2) SIGINT       3) SIGQUIT      4) SIGILL
5) SIGTRAP      6) SIGABRT      7) SIGBUS       8) SIGFPE
9) SIGKILL     10) SIGUSR1     11) SIGSEGV     12) SIGUSR2
13) SIGPIPE    14) SIGALRM     15) SIGTERM     16) SIGSTKFLT
17) SIGCHLD    18) SIGCONT     19) SIGSTOP     20) SIGTSTP
21) SIGTTIN    22) SIGTTOU     23) SIGURG      24) SIGXCPU
25) SIGXFSZ    26) SIGVTALRM   27) SIGPROF     28) SIGWINCH
29) SIGIO      30) SIGPWR      31) SIGSYS      34) SIGRTMIN
35) SIGRTMIN+1 36) SIGRTMIN+2 37) SIGRTMIN+3  38) SIGRTMIN+4
39) SIGRTMIN+5 40) SIGRTMIN+6 41) SIGRTMIN+7  42) SIGRTMIN+8
43) SIGRTMIN+9 44) SIGRTMIN+10 45) SIGRTMIN+11 46) SIGRTMIN+12
47) SIGRTMIN+13 48) SIGRTMIN+14 49) SIGRTMIN+15 50) SIGRTMAX-14
51) SIGRTMAX-13 52) SIGRTMAX-12 53) SIGRTMAX-11 54) SIGRTMAX-10
55) SIGRTMAX-9  56) SIGRTMAX-8  57) SIGRTMAX-7  58) SIGRTMAX-6
59) SIGRTMAX-5  60) SIGRTMAX-4  61) SIGRTMAX-3  62) SIGRTMAX-2
63) SIGRTMAX-1  64) SIGRTMAX  

```

________________________________________________________________________________________________

## `kill -SIGHUP`

kill a process with a `specific signal` (`process ID`)

```bash
[bob@centos-host ~]$ kill -SIGHUP 3214
```

## `kill -9`

kill a process with `signal number` (`process ID`)

```bash
[bob@centos-host ~]$ kill -9 3214
```

## `pkill -KILL`

kill a process (`process Name`)

```bash
[bob@centos-host ~]$ pkill -KILL bash
```

________________________________________________________________________________________________


## run program in background `&`



```bash
sleep 300 &
```

________________________________________________________________________________________________


## `jobs`

to see processes running in the background

```bash
jobs
```

________________________________________________________________________________________________

## `fg`

bring the process from background to foreground (`job number`)

```bash
fg 1
```

________________________________________________________________________________________________

## `pgrep -a bash`

to see which file and directories a process is using. ex: bash

```bash
[bob@centos-host ~]$ pgrep -a bash

8401  bash
```

## `lsof -p 8401`

```bash
[bob@centos-host ~]$ lsof -p 8401
```

________________________________________________________________________________________________


to see what processes are using a file or directory

## `lsof /var/lib/messages`

```bash
[bob@centos-host ~]$ lsof /var/lib/messages
```

________________________________________________________________________________________________
