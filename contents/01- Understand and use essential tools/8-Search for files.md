

## `find`

find [path] [parameters]

without the "path" it will search in the current directory


________________________________________________________________________________________________

## `find -name`


find by name and regex

```bash
[bob@centos-host ~]$ find /tmp/ -name '*.jpg'
/tmp/4.jpg
/tmp/2.jpg
/tmp/3.jpg
/tmp/1.jpg
```

________________________________________________________________________________________________


## `find -size`

find files bigger that 10 Gig

```bash
[bob@centos-host ~]$ find /tmp -size +10G
```

   `bigger` than  `+`
   
   `smaller` than `-`

- `c`   `bytes`


- `k`   `kilobytes`


- `M`   `megabytes`


- `G`   `gigabytes`

________________________________________________________________________________________________


## `find -iname`



find with name (key insensitive) -i

```bash
[bob@centos-host ~]$ touch Hamid
[bob@centos-host ~]$ touch hamid
[bob@centos-host ~]$ touch HAMID

[bob@centos-host ~]$ find -iname hamid
./HAMID
./hamid
./Hamid
```

________________________________________________________________________________________________


it can be any charachter or nothing!

```bash
[bob@centos-host ~]$ find -name 'h*'
./hh
./hamid
./hammmmid
```

________________________________________________________________________________________________


## `find -mmin`



Search by the time `modified`:

search for files that modified in `exact` 5 `minutes` ago (only on that on minute)

```bash
bob@centos-host ~]$ find -mmin 5
```

________________________________________________________________________________________________


search for the files which are modified in `less than` 5 `minutes` (from 5 mins ago until now)

```bash
bob@centos-host ~]$ find -mmin -5
```

________________________________________________________________________________________________



## `find -mtime`




search by time of modified in the `last` `day`!

search for files that are modified in `last` `24 hours`:

```bash
[bob@centos-host ~]$ find -mtime 0
```

________________________________________________________________________________________________



search for files that are modified from 48 hours ago to 24 hours ago:

```bash
[bob@centos-host ~]$ find -mtime 1
```

________________________________________________________________________________________________


### Modified != Changed 

`Modified` : when the `Contents` of a file have been modified

`Changed` :  when the `Metadata` of a file has been changed



----------


min --> Minute

- mmin --> Modified Minute

- cmin --> Changed Minute



time --> day

- mtime --> Modified Day

- ctime --> Changed Day


________________________________________________________________________________________________



## `find -cmin`



Search by the changed time:

search for files that is changed in `less than` 5 `minutes` (from 5 mins ago until now)

```bash
bob@centos-host ~]$ find -cmin -5
```

________________________________________________________________________________________________


### multiple parameters:

by default it's `AND` operator:

```bash
[bob@centos-host ~]$ find -name "h*" -size +10M
```

but, we can use  `-o`  for `OR` operator:

```bash
[bob@centos-host ~]$ find -name "h*" -o -size +10M
```


________________________________________________________________________________________________


find all files that their name does NOT start with h :

`NOT`:

- `-not`

- `\!` and `!`


```bash
[bob@centos-host ~]$ find -not -name "h*"
```

```bash
[bob@centos-host ~]$ find \! -name "h*"
```

________________________________________________________________________________________________

## `find -perm`

### search based of the permission of a file:

search for the `exact` `permission`:

```bash
[bob@centos-host ~]$ find -perm 644
```

```bash
[bob@centos-host ~]$ find -perm u=rw,g=rw,o=r
```

________________________________________________________________________________________________


search for the at least (`minimum`) `permission`: `-` (this permission or higher)

```bash
[bob@centos-host ~]$ find -perm -644
```


```bash
[bob@centos-host ~]$ find -perm -u=rw,g=rw,o=r
```


________________________________________________________________________________________________


search for the `ANY` of these `permissions`: `/` (it will be shown in the search if any of these permissions match)

```bash
[bob@centos-host ~]$ find -perm /644
```

________________________________________________________________________________________________


Find files/directories under /var/log/ directory that the group can write to, but others cannot read or write to it. 


```bash
sudo find /var/log/ -perm -g=w ! -perm /o=rw
```

the "-" means that "at least" : Permissions for the group have to be at least w

the "/" means "ANY" and "!" means "NOT": Permissions for others have not to be neither r nor w. That means, if any of these two permissions, r or w match for others, the result has to be excluded.

________________________________________________________________________________________________


Find our secret file under /home/bob. You can either look for a file that is exactly 213 kilobytes or a file that has permission 402 in octal.

```bash
find /home/bob -size 213k -o -perm 402
```

________________________________________________________________________________________________

## `find -type`

- `f` file

- `d` directory

Find all the files whose permissions are 0777 in /var directory.

```bash
[bob@centos-host ~]$ sudo find /var -type f -perm 0777 
```

________________________________________________________________________________________________


Find all files between 5mb and 10mb in the /usr directory

```bash
[bob@centos-host ~]$ sudo find /usr -type f -size +5M -size -10M
```

________________________________________________________________________________________________


Find cats.txt file under bob's home directory and copy it into /opt directory. `-exec`




```bash
[bob@centos-host ~]$ sudo find /home/bob -type f -name cats.txt -exec cp {} /opt/ \;
```

________________________________________________________________________________________________


## `find -user`

find the files owned by the user root in the “/usr/bin” and copy the files into the “/find/rootfiles/” directory. `-user`



```bash
[bob@centos-host ~]$ sudo find /usr/bin/ -type f -user root -exec cp -rfp {} /find/rootfiles/ \;
```

- -r: Copy directories recursively.
- -f: Force copy by overwriting existing files without prompting.
- -p: Preserve the file attributes such as timestamps, ownership, and permissions.


________________________________________________________________________________________________

## `find -empty -delete`


find and delete all empty files in /tmp. `-empty` `-delete`



```bash
[bob@centos-host ~]$ sudo find /tmp -type f -empty -delete
```

________________________________________________________________________________________________


## `find -uid`

find the files owned by root and with the SUID bit set in /usr.`-uid`



```bash
[bob@centos-host ~]$ sudo find /usr -uid 0 -perm -4000
```


________________________________________________________________________________________________


The RHCSA exam topic **"find"** refers to using the **`find`** command to search for files and directories based on a variety of criteria, such as file name, size, type, permissions, and timestamps. It is an important tool in system administration as it allows you to locate files quickly and efficiently across the filesystem.

Here's what you need to know at the RHCSA exam level, with examples similar to real-world tasks you might encounter during the exam.

---

### **What You Need to Know:**
1. **Basic Usage of `find`**: How to search for files and directories using various criteria.
2. **Finding Files by Name**: Search for files based on their names (exact matches or patterns).
3. **Finding Files by Size**: Search for files that meet specific size conditions (larger, smaller, or exact size).
4. **Finding Files by Time**: Search for files modified, accessed, or changed within a certain timeframe.
5. **Combining Find with Actions**: Execute commands on the found files, such as deleting, copying, or changing permissions.
6. **Permissions and Ownership**: Find files based on ownership or permissions.
7. **File Types**: Search for regular files, directories, symbolic links, etc.
8. **Persistent Usage**: The command itself doesn't make changes that need persistence, but you can combine it with commands that modify files or directories persistently (like `chmod`, `chown`, or `rm`).

---

### **1. Basic Usage of `find`**

The general syntax of the `find` command is:

```bash
find [path] [options] [expression]
```

- **`path`**: Where to search (e.g., `/home`, `/var`, `.` for current directory).
- **`options` and `expression`**: Specify the search criteria (e.g., name, size, type).

#### **Example Task 1: Find All Files in `/etc`**

**Task**: Find all files in the `/etc` directory and its subdirectories.

```bash
find /etc
```

**Explanation**:
- This command will list all files and directories within `/etc` recursively.

---

### **2. Finding Files by Name**

You can search for files or directories by their exact name or by using wildcards.

#### **Example Task 2: Find All Files Named `passwd`**

**Task**: Search the entire file system for files named `passwd`.

```bash
sudo find / -name passwd
```

#### **Example Task 3: Find Files with `.log` Extension**

**Task**: Find all `.log` files in `/var/log`.

```bash
find /var/log -name "*.log"
```

**Explanation**:
- **`-name passwd`** looks for files named `passwd`.
- **`-name "*.log"`** searches for all files ending with `.log`.
- The `*` is a wildcard that represents any string of characters.

---

### **3. Finding Files by Size**

The **`-size`** option allows you to search for files based on their size. You can specify sizes in kilobytes (k), megabytes (M), gigabytes (G), and other units.

#### **Example Task 4: Find Files Larger than 100MB**

**Task**: Find all files larger than **100MB** in the `/home` directory.

```bash
find /home -size +100M
```

#### **Example Task 5: Find Files Smaller than 10KB**

**Task**: Find all files smaller than **10KB** in `/tmp`.

```bash
find /tmp -size -10k
```

**Explanation**:
- **`-size +100M`** searches for files larger than 100 megabytes.
- **`-size -10k`** searches for files smaller than 10 kilobytes.
- The `+` indicates "larger than", and `-` indicates "smaller than."

---

### **4. Finding Files by Time**

You can use the **`-mtime`**, **`-atime`**, or **`-ctime`** options to find files based on modification time, access time, or change time.

- **`-mtime`**: Modified time.
- **`-atime`**: Accessed time.
- **`-ctime`**: Changed time (metadata).

#### **Example Task 6: Find Files Modified in the Last 7 Days**

**Task**: Find files in `/var` modified in the last **7** days.

```bash
find /var -mtime -7
```

#### **Example Task 7: Find Files Not Accessed in the Last 30 Days**

**Task**: Find files in `/home` that haven't been accessed in **30** days.

```bash
find /home -atime +30
```

**Explanation**:
- **`-mtime -7`** finds files modified in the last 7 days.
- **`-atime +30`** finds files that have not been accessed in over 30 days.

---

### **5. Combining `find` with Actions**

Once you've found files, you can execute commands on them. For example, you can delete files, change their ownership, or modify their permissions.

#### **Example Task 8: Find and Delete Empty Files**

**Task**: Find and delete all empty files in `/tmp`.

```bash
find /tmp -type f -empty -delete
```

#### **Example Task 9: Find All `.log` Files and Change Their Permissions**

**Task**: Find all `.log` files in `/var/log` and change their permissions to `644`.

```bash
find /var/log -name "*.log" -exec chmod 644 {} \;
```

**Explanation**:
- **`-type f -empty`** finds empty regular files.
- **`-delete`** removes the found files.
- **`-exec`** allows you to run a command (`chmod` in this case) on the found files. The `{}` represents the current file, and `\;` ends the command.
  
---

### **6. Finding Files by Permissions or Ownership**

You can find files based on their **permissions** or **ownership** (user or group).

#### **Example Task 10: Find Files Owned by `root`**

**Task**: Find all files in `/etc` owned by `root`.

```bash
find /etc -user root
```

#### **Example Task 11: Find Files with `777` Permissions**

**Task**: Find files with **777** permissions in `/home`.

```bash
find /home -perm 777
```

**Explanation**:
- **`-user root`** searches for files owned by the user `root`.
- **`-perm 777`** finds files with full read, write, and execute permissions for all users.

---

### **7. Finding Specific File Types**

The **`-type`** option allows you to search for different types of files, such as:
- **`-type f`**: Regular files.
- **`-type d`**: Directories.
- **`-type l`**: Symbolic links.

#### **Example Task 12: Find All Directories in `/var`**

**Task**: Find all directories in the `/var` directory.

```bash
find /var -type d
```

#### **Example Task 13: Find All Symbolic Links in `/usr`**

**Task**: Find all symbolic links in `/usr`.

```bash
find /usr -type l
```

**Explanation**:
- **`-type d`** finds directories.
- **`-type l`** finds symbolic links.

---

### **8. Search Using Multiple Criteria**

You can combine multiple criteria to narrow your search.

#### **Example Task 14: Find `.log` Files Larger than 1MB**

**Task**: Find all `.log` files in `/var/log` that are larger than **1MB**.

```bash
find /var/log -name "*.log" -size +1M
```

#### **Example Task 15: Find Files Owned by `apache` and Modified in the Last 7 Days**

**Task**: Find files in `/var/www` owned by the `apache` user and modified in the last **7** days.

```bash
find /var/www -user apache -mtime -7
```

**Explanation**:
- **`-name "*.log" -size +1M`** finds `.log` files larger than 1MB.
- **`-user apache -mtime -7`** finds files owned by `apache` that were modified in the last 7 days.

---

### **Summary of Skills for RHCSA Exam on the `find` Command:**
1. **Find files by name** using `-name`.
2. **Search by size** with `-size` and units like `k`, `M`, `G`.
3. **Search by time** using `-mtime`, `-atime`, and `-ctime`.
4. **Execute actions** like `delete` or `chmod` on found files using `-exec`.
5. **Find files based on permissions** using `-perm`.
6. **Locate files by ownership** using `-user` and `-group`.
7. **Search by file type** with `-type` (files, directories, symbolic links).

Practicing these commands on RHEL 9 will help you master file searching for the RHCSA exam. Remember to make changes persistent only when needed (e.g., with `chmod` or `rm` actions).
