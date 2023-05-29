

everythin that starts with R

```bash
[bob@centos-host ~]$ grep '^R' myfile.txt 
Resolving rpmfind.net (rpmfind.net)... 195.220.108.108
```

________________________________________________________________________________________________


everythin that ends with rpm

```bash
[bob@centos-host ~]$ grep 'rpm$' myfile.txt 
[bob@centos-host ~]$ wget http://rpmfind.net/linux/centos/8.4.2105/BaseOS/x86_64/os/Packages/iputils-20180629-7.el8.x86_64.rpm
```

________________________________________________________________________________________________


dot (.) matchs any character (each dot only one char)

```bash
[bob@centos-host ~]$ grep 'c.t' myfile.txt
cat sdf asdf asdf
cut asdf asdf asdf
```



```bash
[bob@centos-host ~]$ sudo grep -r 'c..t' /etc/os-release 
ID="centos"
CPE_NAME="cpe:/o:centos:centos:8"
HOME_URL="https://centos.org/"
```

________________________________________________________________________________________________


look for a . with escape character

```bash
[bob@centos-host ~]$ sudo grep '\.' /etc/os-release 
HOME_URL="https://centos.org/"
BUG_REPORT_URL="https://bugzilla.redhat.com/"
```

________________________________________________________________________________________________



### * match previous element 0 or more times

```bash
[bob@centos-host ~]$ sudo grep -r 'let*' /etc/ 
```

________________________________________________________________________________________________


starts and ends with / and 0 or more chars between them:

```bash
[bob@centos-host ~]$ sudo grep -r '/.*/' /etc/
```

________________________________________________________________________________________________


### + match previous element 1 or more times

```bash
[bob@centos-host ~]$ sudo grep -r 'let\+' /etc/ 
```

________________________________________________________________________________________________


# to avoid confusing using \ we can use egrep or grep -E

________________________________________________________________________________________________


 #### + match previous element 1 or more times

```bash
[bob@centos-host ~]$ sudo grep -Er 'let+' /etc/ 
```

________________________________________________________________________________________________


at least 3 zeros

```bash
[bob@centos-host ~]$ sudo egrep -r '0{3,}' /etc/ 
```

we use {} to determine the number of repetitions

________________________________________________________________________________________________


at most 3 zeros

```bash
[bob@centos-host ~]$ sudo egrep -r '0{,3}' /etc/ 
```

________________________________________________________________________________________________


 #### ? match previous element 0 or 1 time (make it optional)

```bash
[bob@centos-host ~]$ sudo grep -Er '0?' /etc/ 
```

________________________________________________________________________________________________


it matches "disable" and "disabled"

```bash
[bob@centos-host ~]$ sudo egrep -r 'disabled?' /etc/
```

________________________________________________________________________________________________


### OR |

```bash
[bob@centos-host ~]$ sudo egrep -r 'disabled?|enabled?' /etc/
```

________________________________________________________________________________________________


[a-z]  match any lowercase char

[A-Z]       match any uppercase char

[0-9]       match any number

[hs3nmg]    match any one character of them


________________________________________________________________________________________________


match cat or cut

```bash
[bob@centos-host ~]$ sudo egrep -r 'c[au]t' /etc/
```

________________________________________________________________________________________________


### Subexpression

```bash
[bob@centos-host ~]$ sudo egrep -r '/dev/(([a-z]|[A-Z])*[0-9]?)*' /etc/
```

________________________________________________________________________________________________


links without encryption

[^s]      --->      not followed by s

```bash
[bob@centos-host ~]$ sudo egrep -r 'http[^s]' /etc/
```

________________________________________________________________________________________________


find any upper case letter

```bash
[bob@centos-host ~]$ sudo egrep -r '/[^a-z]' /etc/
```

________________________________________________________________________________________________


While ignoring the case sensitivity, change all values disabled to enabled in /home/bob/values.conf config file.

```bash
[bob@centos-host ~]$ sed -i 's/disabled/enabled/gi' /home/bob/values.conf
```

________________________________________________________________________________________________


Change all values enabled to disabled in /home/bob/values.conf config file from line number 500 to 2000.

```bash
[bob@centos-host ~]$ sed -i '500,2000s/enabled/disabled/g' /home/bob/values.conf
```

________________________________________________________________________________________________


Replace all occurrence of string #%$2jh//238720//31223 with $2//23872031223 in /home/bob/data.txt file.

```bash
[bob@centos-host ~]$ sed -i 's~#%$2jh//238720//31223~$2//23872031223~g' /home/bob/data.txt
```

________________________________________________________________________________________________


Filter out the lines that contain any word that starts with a capital letter and are then followed by exactly two lowercase letters in /etc/nsswitch.conf file 

```bash
[bob@centos-host ~]$ egrep -w '[A-Z][a-z]{2}' /etc/nsswitch.conf
```

________________________________________________________________________________________________


###   Basic vs Extended Regular Expressions

       In basic regular expressions the meta-characters ?, +, {, |, ( and ) lose their special meaning;
       
       instead use the backslashed versions \?, \+, \{, \|, \(, and \).
