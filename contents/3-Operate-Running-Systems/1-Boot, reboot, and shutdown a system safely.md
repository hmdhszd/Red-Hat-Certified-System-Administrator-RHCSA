


## `systemctl reboot`

```bash
systemctl reboot
```

```bash
systemctl reboot --force
```

________________________________________________________________________________________________


## `systemctl poweroff`


```bash
systemctl poweroff
```


```bash
systemctl poweroff --force
```

________________________________________________________________________________________________


this command is like pressing power button


```bash
systemctl poweroff --force --force
```



________________________________________________________________________________________________


## `shutdown`

`00:00` - `23:59`

```bash
shutdown 02:00
```

________________________________________________________________________________________________


shutdown after 15 minutes

```bash
shutdown +15
```

________________________________________________________________________________________________


reboot


```bash
shutdown -r +15
```

```bash
shutdown -r 02:00
```


________________________________________________________________________________________________


wall message


```bash
shutdown -r +15 'this machine will be restarted for updating purposes in 15 minutes'
```

________________________________________________________________________________________________
