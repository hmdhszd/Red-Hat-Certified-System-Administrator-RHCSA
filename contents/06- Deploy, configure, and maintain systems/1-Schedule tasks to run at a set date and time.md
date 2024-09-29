

## `cron` vs `anacron`

anacron will run the command if the computer is `turned off`, whenever it's on again. cron don't!

the smallest unit in `anacron` is `DAY`.



________________________________________________________________________________________________


## Edit `/etc/crontab` VS `crontab -e` command


Yes, you can use both methods to set up cron jobs in Red Hat Linux, but they are used for different purposes.

1. **Editing `/etc/crontab`:**
   - This file is the `system-wide` crontab file and can be edited directly to add cron jobs.
   - It allows you to schedule jobs to be run by `specific` `users`.
   - Each line in `/etc/crontab` has an additional field specifying the `user` who will run the command. The format is:
     ```
     * * * * * user command
     ```
   - For example, to run a script every day at midnight as the user `username`, you would add:
     ```
     0 0 * * * username /path/to/script.sh
     ```

2. **Using `crontab -e`:**
   - This command opens the crontab file for the `current user` in an editor.
   - The jobs you add here will run as the user who edited the crontab.
   - This method is useful for setting up personal cron jobs and doesn't require specifying the user in the cron entry.
   - For example, using `crontab -e` and adding:
     ```
     0 0 * * * /path/to/script.sh
     ```
     will run `/path/to/script.sh` as the current user every day at midnight.

In summary:
- Use `/etc/crontab` if you need to set up cron jobs that run under `different user` accounts or need to be `managed at the system level`.
- Use `crontab -e` for `user-specific` cron jobs that run as the user who created them.


________________________________________________________________________________________________

# Cron


```bash
A    B    C    D    E    command and arguments


filed			  Meaning			  values
A			  minute			  0-59
B			  hour				  0-23
C			  day of month			  1-31
D			  month			          1-12 (or names)
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

Anacron is primarily `system-wide`, meaning it is generally used for scheduling tasks that should `run for all users` on the system.

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



## Configure the atd service to specifically grant access to the user Adam while denying access to the user Tom.


Explanation
1. Prioritize at.allow for Security:

#### Use the `/etc/at.allow` file to explicitly list those granted access, as it takes precedence over `/etc/at.deny`.

#### This enhances security by defaulting to denial for users not explicitly listed in at.allow.


```bash
echo "Adam" > /etc/at.allow
echo "Tom" > /etc/at.deny

systemctl reload atd
```


#### Verify Access:

As Adam, try scheduling a job using at:

```bash
$ at now + 5 minutes

at> echo "This is a test job."

at> <EOT>

As Tom, attempt the same and confirm access is denied.
```



________________________________________________________________________________________________


## prevent all users from using the crontab command except tom.



```bash
echo tom >> /etc/cron.allow
```


________________________________________________________________________________________________

## schedule a cron job that prints "Break Time!" Every two hours on weekdays on your current screen.


the first pseudo-terminal --> (`/dev/pts/0`)

```bash
crontab -e

00 */2 * * 1-5 echo Break Time! > /dev/pts/0
```


________________________________________________________________________________________________





Here are some **additional example tasks** from the **"Scheduling and Managing Tasks"** topic:

---

### 1. **Schedule a cron job for user backup**
- **Task:** Create a cron job for the user `james` that runs every Friday at 6:00 PM and executes the `/usr/local/bin/backup.sh` script.
  - **Steps:**
    1. Edit the user-specific crontab for `james`:
       ```bash
       sudo crontab -u james -e
       ```
    2. Add the following entry to schedule the job:
       ```
       0 18 * * 5 /usr/local/bin/backup.sh
       ```

---

### 2. **Create a one-time `at` job**
- **Task:** Create a one-time `at` job that runs the script `/usr/local/bin/cleanup.sh` at 9:00 PM today.
  - **Steps:**
    1. Use the `at` command to schedule the job:
       ```bash
       echo "/usr/local/bin/cleanup.sh" | at 9:00 PM
       ```

---

### 3. **Schedule a cron job using a script**
- **Task:** Create a shell script `/usr/local/bin/run-script.sh` that outputs the current date and time to `/var/log/time.log` and set up a cron job to run this script every day at 4:30 AM.
  - **Steps:**
    1. Create the script:
       ```bash
       echo 'date >> /var/log/time.log' > /usr/local/bin/run-script.sh
       chmod +x /usr/local/bin/run-script.sh
       ```
    2. Edit the crontab:
       ```bash
       crontab -e
       ```
    3. Add the following entry:
       ```
       30 4 * * * /usr/local/bin/run-script.sh
       ```

---

### 4. **Create a systemd service and timer for weekly cleanup**
- **Task:** Create a systemd service and timer that runs `/usr/local/bin/weekly-cleanup.sh` every Monday at 2:00 AM.
  - **Steps:**
    1. Create the service file `/etc/systemd/system/weekly-cleanup.service`:
       ```bash
       [Unit]
       Description=Weekly Cleanup

       [Service]
       ExecStart=/usr/local/bin/weekly-cleanup.sh
       ```
    2. Create the timer file `/etc/systemd/system/weekly-cleanup.timer`:
       ```bash
       [Unit]
       Description=Runs weekly cleanup every Monday at 2:00 AM

       [Timer]
       OnCalendar=Mon 02:00
       Persistent=true

       [Install]
       WantedBy=timers.target
       ```
    3. Enable and start the timer:
       ```bash
       systemctl enable --now weekly-cleanup.timer
       ```

---

### 5. **Modify an existing cron job**
- **Task:** A cron job currently runs the `/usr/local/bin/report.sh` script every day at 6:00 AM. Modify this job so that it runs every 2 days at the same time.
  - **Steps:**
    1. Edit the crontab:
       ```bash
       crontab -e
       ```
    2. Modify the entry for the script:
       ```
       0 6 */2 * * /usr/local/bin/report.sh
       ```

---

### 6. **Remove an existing systemd timer**
- **Task:** Disable and remove a systemd timer called `hourly-cleanup.timer` that runs the `/usr/local/bin/hourly-cleanup.sh` script every hour.
  - **Steps:**
    1. Stop and disable the timer:
       ```bash
       systemctl stop hourly-cleanup.timer
       systemctl disable hourly-cleanup.timer
       ```
    2. Remove the timer and service files:
       ```bash
       rm /etc/systemd/system/hourly-cleanup.timer
       rm /etc/systemd/system/hourly-cleanup.service
       ```
    3. Reload systemd to apply the changes:
       ```bash
       systemctl daemon-reload
       ```

---

### 7. **Create a systemd timer to send system stats**
- **Task:** Create a systemd timer that runs the `/usr/local/bin/sysstats.sh` script every day at 7:30 PM, which sends system statistics to a remote server.
  - **Steps:**
    1. Create the service file `/etc/systemd/system/sysstats.service`:
       ```bash
       [Unit]
       Description=Send system statistics

       [Service]
       ExecStart=/usr/local/bin/sysstats.sh
       ```
    2. Create the timer file `/etc/systemd/system/sysstats.timer`:
       ```bash
       [Unit]
       Description=Run sysstats script daily at 7:30 PM

       [Timer]
       OnCalendar=19:30
       Persistent=true

       [Install]
       WantedBy=timers.target
       ```
    3. Enable and start the timer:
       ```bash
       systemctl enable --now sysstats.timer
       ```

---

### 8. **Run a job using anacron**
- **Task:** Schedule a job using **anacron** to run a script `/usr/local/bin/monthly-maintenance.sh` once a month, ensuring that if the system is off, the job will run the next time the system starts.
  - **Steps:**
    1. Edit the `/etc/anacrontab` file and add an entry:
       ```
       30      10      monthly-maintenance   /usr/local/bin/monthly-maintenance.sh
       ```
       This entry means: Run the script **30 days** after the last run (if missed) with a **10-minute delay** after system startup.

---

### Summary of Key Topics and Skills for "Scheduling and Managing Tasks"

- **Cron jobs**: Know how to schedule tasks using cron syntax for both system-wide and user-specific cron jobs.
- **at jobs**: Understand how to schedule one-time jobs using `at`.
- **systemd timers**: Be able to create, enable, and manage systemd timer units to automate tasks in a modern `systemd` environment.
- **anacron**: Schedule periodic jobs that run even if the system was off during their regular execution time.

These practice tasks cover **critical scheduling topics** in the RHCSA exam. Make sure you're comfortable with creating, modifying, and managing scheduled tasks using these different tools. Hands-on practice with all these scheduling tools will ensure you're prepared for the exam and for real-world system administration scenarios.






________________________________________________________________________________________________
