## `/var/log/cron`

to see the `logs` of `cron` and `anacron` and `at`

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


in the `/etc/crontab` you can find `MAILTO=root`

it sends emails to root if it runs `Successful` or `UnSuccessful`!

we can specify an external email address as well


________________________________________________________________________________________________


## `| systemd-cat --identifier=`

in the "anacron" and "at" you can not see a lot of info in the logs, you should add this at the end of your command

```bash
. . . | systemd-cat --identifier=test_job
```

ex:


```bash
at "now + 1 minute"

echo "this is my at job" | systemd-cat --identifier=test_job
```


```bash
journalctl | grep test_job
```


________________________________________________________________________________________________

## `anacron -n -f`

`force` to run anacron `now`

```bash
sudo anacron -n -f
```

________________________________________________________________________________________________


### to see the logs related to "at" command

```bash
sudo grep atd /ver/log/cron
```

________________________________________________________________________________________________

## `/var/log/syslog`

the logs also can be find here

```bash
/var/log/syslog
```

________________________________________________________________________________________________
