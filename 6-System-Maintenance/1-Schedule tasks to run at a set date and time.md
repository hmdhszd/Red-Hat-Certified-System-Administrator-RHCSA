

`cron` vs `anacron`

anacron will run the command if the computer is `turned off`, whenever it's on again. cron don't!

the smallest unit in `anacron` is `DAY`.



________________________________________________________________________________________________

# Cron


```bash
A    B    C    D    E    command and arguments


filed			  Meaning			  values
A			  minute			  0-59
B			  hour				  0-23
C			  day of month			  1-31
D			  month			          1-12 (or names, see below)
E			  day of week			  0-7 (0 or 7 is Sunday, or use names)
```


use `comma` `,` to have `multiple values`

use `dash` `-` to specify a `range` of the values
 
use `slash` `/` to specify `steps`



________________________________________________________________________________________________


in the cronjob we should use full path

```bash
[bob@centos-host ~]$ which touch

/usr/bin/touch
```

________________________________________________________________________________________________


`e` --> edit 

```bash
crontab -e
```

________________________________________________________________________________________________


`l` --> list

```bash
crontab -l
```

________________________________________________________________________________________________

## `crontab -e -u`

if you wanna edit cron of `another user`:

- login to this user then edit crontab

OR

- use -u at the end of your command


```bash
crontab -e -u hamid
```

________________________________________________________________________________________________


## `crontab -r`

remove/`erase` crontab entirely

```bash
crontab -r
```

________________________________________________________________________________________________


### Daily, Hourly, Monthly, Weekly

put your shell `script` here:

it should have `NO extentions`

the script should have the `permission` of `R` and `X`

- `/etc/cron.daily/`

- `/etc/cron.hourly/`

- `/etc/cron.monthly/`

- `/etc/cron.weekly`



________________________________________________________________________________________________


# AnaCron


## `/etc/anacrontab`

in anacron we don't care when it's run, we just want it to run daily, weekly, monthly...

```bash
cat /etc/anacrontab

period in days     delay in minutes(for run jubs)    job-identifier    command
```


`period in days`      -->     can be `numbers` like 3 / can be `@weekly` / `@monthly`

________________________________________________________________________________________________


## `anacron -n`

force to run anacron jobs `NOW`

```bash
anacron -n
```

________________________________________________________________________________________________



## `anacron -T`

`test` to see if the file edited correctly

```bash
anacron -T
```



________________________________________________________________________________________________


The job-identifier variable specifies a unique name of a job which is used in the log files.

The command variable specifies the command to execute.

The command can either be a command such as "ls /proc >> /tmp/proc" or a command to execute a custom script.



```bash
# /etc/anacrontab: configuration file for anacron
 
# See anacron(8) and anacrontab(5) for details.
 
SHELL=/bin/sh
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root
# the maximal random delay added to the base delay of the jobs
# RANDOM_DELAY=45
# the jobs will be started during the following hours only
START_HOURS_RANGE=3-22
 
#period in days   delay in minutes   job-identifier	      	     command

1	      	    5		      cron.daily		  nice run-parts /etc/cron.daily
7	      	    25		      cron.weekly		  nice run-parts /etc/cron.weekly
@monthly 	    45		      cron.monthly		  nice run-parts /etc/cron.monthly
```




________________________________________________________________________________________________


# at


```bash
at 15:00
```


press  `CTRL + D`  to save the job



________________________________________________________________________________________________




```bash
at 'August 20 2022'
```

________________________________________________________________________________________________




```bash
at '02:30 August 20 2022'
```

________________________________________________________________________________________________




```bash
at 'now + 30 minutes'
```

________________________________________________________________________________________________




```bash
at 'now + 30 hours'
```

________________________________________________________________________________________________




```bash
at 'now + 30 days'
```

________________________________________________________________________________________________




```bash
at 'now + 30 weeks'
```

________________________________________________________________________________________________




```bash
at 'now + 30 months'
```

________________________________________________________________________________________________


## `atq`


to check what jobs are scheduled to run

```bash
atq
```

the first number is `JobNumber`

________________________________________________________________________________________________


## `at -c`

to see what job 20 `contains`

```bash
at -c 20
```

________________________________________________________________________________________________


## `atrm`

to `remove` the job

```bash
atrm 20
```

________________________________________________________________________________________________



### configure a cron job that writes the message "Hello WeekDays!" to the system log file `/var/log/messages` at noon (12 PM) on weekdays only.

```bash
crontab -e

00 12 * * 1-5 logger "Hello WeekDays!"
```



### configure a cron job that writes the message "Hello WeekEnds!" to the system log file `/var/log/messages` at midnight (12 AM) on WeekEnds only.

```bash
crontab -e

00 00 * * 6-7 logger "Hello WeekEnds!"
```




Verify:

```bash
tail -f /var/log/messages
```


`logger` --> `/var/log/messages`

________________________________________________________________________________________________

