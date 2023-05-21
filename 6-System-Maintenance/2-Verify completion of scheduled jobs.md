

to see cron logs

```bash
cat /var/log/cron
```

________________________________________________________________________________________________




```bash
sudo grep CMD /var/log/cron
```

________________________________________________________________________________________________




```bash
sudo grep anacron /var/log/cron
```

________________________________________________________________________________________________


in the /etc/crontab you can find MAILTO=root

it sends emails to root if it's successfully run or not!

we can specify an external email address as well


________________________________________________________________________________________________


in the "anacron" and "at" you can not see a lot of info in the logs, you should add this at the end of your command

```bash
. . . | systemd-cat --identifier=test_job
```

________________________________________________________________________________________________


force to run anacron now

```bash
sudo anacron -n -f
```

________________________________________________________________________________________________


### to see the logs related to "at" command

```bash
sudo grep atd /ver/log/cron
```

________________________________________________________________________________________________

the logs also can be find here

```bash
/var/log/syslog
```

________________________________________________________________________________________________
