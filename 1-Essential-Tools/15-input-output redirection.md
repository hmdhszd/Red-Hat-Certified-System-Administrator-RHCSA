

`standard-output` `1`

```bash
[bob@centos-host ~]$ date 1>standard-output.txt 2>standard-error.txt

[bob@centos-host ~]$ cat standard-output.txt 
Sat May 13 21:06:35 UTC 2023
```

________________________________________________________________________________________________


`standard-error` `2`

```bash
[bob@centos-host ~]$ ping 5555 1>standard-output.txt 2>standard-error.txt

[bob@centos-host ~]$ cat standard-error.txt 
-bash: ping: command not found
```

________________________________________________________________________________________________

`2>&1`

```bash
[bob@centos-host ~]$ ping 5555 > /dev/null 2>&1
```

________________________________________________________________________________________________


`redirect input` `<`

```bash
sendmail hamid@hszd.ir < email.txt
```

________________________________________________________________________________________________


`<<EOF`

```bash
[bob@centos-host ~]$ cat <<EOF

> 3.5.
> 345
> 5
> dfg
> sdf
> t
> dsh
> drh
> EOF

3.5.
345
5
dfg
sdf
t
dsh
drh
```

________________________________________________________________________________________________


`| column -t`

```bash
[bob@centos-host ~]$ cat file 
asdf sdfhdfh
sdgf 3245sdfg
asdf t346234623
fgasrdghasdfgh srdhsdfh4uw45j
serdhzxdve r
s dfghsrdfg se
sdfh54hsbxrreb4
g 4ybntyhsg g4 
g424h hw43
```

```bash
[bob@centos-host ~]$ cat file  | column -t
asdf             sdfhdfh         
sdgf             3245sdfg        
asdf             t346234623      
fgasrdghasdfgh   srdhsdfh4uw45j  
serdhzxdve       r               
s                dfghsrdfg       se
sdfh54hsbxrreb4                  
g                4ybntyhsg       g4
g424h            hw43    
```

________________________________________________________________________________________________

`<<<`

calculate



```bash
[bob@centos-host ~]$ bc <<<1+2+3+4

10 
```
