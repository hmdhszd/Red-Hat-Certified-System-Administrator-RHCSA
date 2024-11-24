


‍`wheel` group     -->     `root` `permission`

docker group    -->     manage docker containers 

a user have one primary group, but can be in several other groups

________________________________________________________________________________________________





```bash
[bob@centos-host ~]$ sudo useradd hamid
[bob@centos-host ~]$ sudo groupadd developers
```

________________________________________________________________________________________________


# add a user to a group


## `gpasswd --add`

```bash
[bob@centos-host ~]$ sudo gpasswd --add hamid developers

Adding user hamid to group developers
```

OR

## `gpasswd -a`

```bash
[bob@centos-host ~]$ sudo gpasswd -a hamid developers

Adding user hamid to group developers
```


OR

## `usermod -aG`

append new `secondary` group to the list of groups of a user

```bash
[bob@centos-host ~]$ sudo usermod -aG developers hamid
```

OR

## `usermod -g`

change the `primary` group

```bash
[bob@centos-host ~]$ sudo usermod -g developers hamid
```

________________________________________________________________________________________________




## `groups`


```bash
[bob@centos-host ~]$ groups hamid

hamid : hamid developers
```

________________________________________________________________________________________________




# `remove` a user from a group



## `gpasswd --delete`


```bash
[bob@centos-host ~]$ sudo gpasswd --delete hamid developers

Removing user hamid from group developers
```

OR

## `gpasswd -r`


```bash
[bob@centos-host ~]$ sudo gpasswd -r hamid developers

Removing user hamid from group developers
```

________________________________________________________________________________________________


## `groupmod --new-name`

### change the name of a group


```bash
[bob@centos-host ~]$ sudo groupmod --new-name programers developers
```



```bash
[bob@centos-host ~]$ cat /etc/group | grep programers

programers:x:1004:hamid
```

________________________________________________________________________________________________



## `groupdel`

delete a primary group

```bash
[bob@centos-host ~]$ sudo groupdel programers
```

________________________________________________________________________________________________




### Create the following users and groups, configure their permissions for specific directories, and ensure appropriate file ownership for newly created files.



#### Users:

- amr and biko (members of the "admins" group)

- carlos and david (members of the "developers" group)



```bash
groupadd admins

groupadd developers
```

```bash
useradd amr
usermod -aG admins amr
```

```bash
useradd biko
usermod -aG admins biko
```

```bash
useradd carlos
usermod -aG developers carlos
```

```bash
useradd david
usermod -aG developers david
```




#### Directories:

- /admins (accessible only to owner and admins group members, owned by biko)

- /developers (accessible only to developers group members, owned by carlos)


```bash
mkdir /admins

mkdir /developers
```

```bash
chown biko:admins /admins

chmod 770 /admins # Owner and group full access, no access for others
```


```bash
chown carlos:developers /developers

chmod 770 /developers # Owner and group full access, no access for others
```




#### File Ownership:

- `New` files in /developers or /admins should be owned by the respective group owner. (setgid)

- Only file creators should be allowed to delete their files. (sticky bit)



```bash
chmod o+t,g+s /admins # Set sticky bit and setgid bit
```



```bash
chmod o+t,g+s /developers # Set sticky bit and setgid bit
```




________________________________________________________________________________________________


### Create a directory named "/collaboration", and configure it so that any files or subdirectories `created` within that directory are owned by the group "managers".



```bash
mkdir /collaboration

groupadd managers

chown :managers /collaboration

chmod g+s /collaboration
```


Note that:

SGID - if set on a file, it allows the file to be executed as the group that owns the file (similar to SUID).

SGID - if set on a directory, any files created in the directory will have their group ownership set to that of the directory owner.


________________________________________________________________________________________________



To create a shared directory where two groups have full access (read, write, and execute) but cannot delete files, you can use Linux filesystem permissions with the **sticky bit** and **group ownership**. Here's how to achieve this:

### Step 1: Create the shared directory

1. **Create the directory** that you want to share between the groups:
   ```bash
   sudo mkdir /shared_directory
   ```

### Step 2: Create the two groups

1. **Create the groups** if they don't already exist:
   ```bash
   sudo groupadd group1
   sudo groupadd group2
   ```

2. **Add users to the groups** (replace `user1`, `user2`, etc., with actual usernames):
   ```bash
   sudo usermod -aG group1 user1
   sudo usermod -aG group2 user2
   ```

### Step 3: Set the group ownership of the directory

1. **Change the group ownership** of the shared directory to one of the groups (e.g., `group1`). You can assign both groups by setting the **SGID** later:
   ```bash
   sudo chown :group1 /shared_directory
   ```

2. **Ensure both groups can access the directory** by adjusting the permissions:
   ```bash
   sudo chmod 770 /shared_directory
   ```

   This command gives read, write, and execute permissions to both the owner and the group, and no access to others.

### Step 4: Set the SGID and Sticky Bit

1. **Set the SGID (Set Group ID)** on the directory so that all new files and subdirectories inherit the group ownership of the directory:
   ```bash
   sudo chmod g+s /shared_directory
   ```

2. **Set the sticky bit** to prevent users from deleting or renaming files they do not own:
   ```bash
   sudo chmod +t /shared_directory
   ```

### Step 5: Modify ACLs (Access Control Lists)

1. **Set ACLs to give full access to both groups**, ensuring they have read, write, and execute access:
   ```bash
   sudo setfacl -m g:group1:rwx /shared_directory
   sudo setfacl -m g:group2:rwx /shared_directory
   ```

2. **Ensure all files inherit the same permissions** by setting default ACLs:
   ```bash
   sudo setfacl -d -m g:group1:rwx /shared_directory
   sudo setfacl -d -m g:group2:rwx /shared_directory
   ```

### Step 6: Verify the setup

1. **Check the permissions** on the directory to ensure everything is set up correctly:
   ```bash
   ls -ld /shared_directory
   getfacl /shared_directory
   ```

### How This Works

- **SGID** ensures that any file created in the directory will have the same group ownership as the directory itself (in this case, `group1`).
- **Sticky bit** (`+t`) prevents users from deleting or renaming files they do not own, even if they have write access to the directory. This allows both groups to write files, but users can only delete their own files.
- **ACLs** provide fine-grained control to ensure both `group1` and `group2` have full read, write, and execute permissions.

### Example Permissions

After setting up, the directory `/shared_directory` will have permissions like this:

```
drwxrws-t 2 root group1 4096 Nov 22 10:00 /shared_directory
```

This indicates:
- `rwxrws-t`: The directory has full access for the owner and group, with the SGID (`s`) and sticky bit (`t`) set.
- Both `group1` and `group2` can fully access the directory, but they cannot delete files they do not own.

This setup ensures that the two groups can collaborate, write files, but cannot delete each other's files.


