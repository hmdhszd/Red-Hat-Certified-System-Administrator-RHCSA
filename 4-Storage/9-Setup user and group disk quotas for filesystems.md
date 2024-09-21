
### What is Disk Quota?

Disk quota is a way to **limit the amount of disk space** and the number of inodes (files) that users or groups can consume on a file system. This is important in multi-user environments to prevent one user from using up all the disk space.

### Key Concepts You Need to Know:
1. **Enabling Quotas on a Filesystem:**
   - Quotas must be enabled on a per-filesystem basis. You must edit `/etc/fstab` to enable quotas.
   
2. **Quota Types:**
   - **`User Quotas:`** Limit the amount of space a user can use.
   - **`Group Quotas:`** Limit the amount of space a group can use.

3. **Quota Limits:**
   - **`Soft Limit:`** Users can `exceed` this limit `temporarily`, usually with a `grace period`.
   - **`Hard Limit:`** Users `cannot exceed` this limit. It is strictly enforced.
   
4. **Grace Periods:** 
   - When users exceed their soft limit, they are given a certain amount of time (the grace period) to reduce their usage before it becomes a hard limit.

5. **Managing Quotas:**
   - Key commands:
     - `quota`: Check the usage and limits for a user.
     - `repquota`: Report on the quotas for a filesystem.
     - `edquota`: Edit the quotas for a user or group.
     - `setquota`: Set the quotas directly from the command line.

### What You Need to Know for the RHCSA Exam:

1. **Enabling and Configuring Quotas:**
   - You will need to know how to configure user and group quotas on a specific filesystem.

#### Step-by-Step Example:
1. **Install Quota Tools (if not already installed):**
   ```bash
   sudo yum install quota
   ```

2. **Modify `/etc/fstab` to enable quota on a filesystem:**

   change `/etc/fstab` to enable the quota (`usrquota` , `grpquota`)

   ```bash
   nano /etc/fstab

   /dev/sdb1 /mybackups     xfs    defaults,usrquota,grpquota  0 2
   ```




   2-1. **enforce quota on `ext4` filesystem needs one more commands:**

      this will create 2 files on the filesystem: `aquata.group` and `aquata.user`

      in these files, the system `keeps` the `track` of how much data the user or group is using
   
      You need to initialize the quota files with: (`-cug` = `--create-files --user --group`)

      ```bash
      quotacheck -cug /dev/sdb1
      ```
       
      OR
      
      ```bash
      quotacheck --create-files --user --group /dev/sdb1
      ```



3. **Reboot OR Remount the Filesystem:**

   ```bash
   systemctl reboot 
   ```
   OR
   
   ```bash
   sudo mount -o remount /
   ```


4. **Turn On Quotas:**

   ```bash
   quotaon -v /mybackups
   ```


5. **Set Quotas for a User:**
   To set quotas for a specific user (e.g., `john`), use the following command:
   ```bash
   sudo edquota john
   ```
   This will open a text editor where you can set soft and hard limits for blocks (disk space) and inodes (number of files).

   Example:
   ```
   Disk quotas for user john (uid 1001):
     Filesystem     blocks   soft   hard    inodes   soft   hard
     /dev/sda1          0     1000   1500       0       0       0
   ```

6. **Check the Quota Usage:**
   Use the `quota` command to check the user’s quota usage:
   ```bash
   quota john
   ```

7. **Set Grace Periods (Optional):**
   You can set the grace period for soft limits using `edquota -t` to set a grace period for all users.


8. **Test Quota (Optional):**

```bash
fallocate --length 100M /mybackups/hamid/100Mfile
```




#### Practice Scenario:
> **Practice Task**: You are asked to configure quotas for user **`alice`** on the `/home` partition so that she has a **soft limit of 500MB** and a **hard limit of 600MB** for disk usage. Also, set a **soft limit of 200 files** and a **hard limit of 250 files**.

#### Steps:
1. Ensure quota tools are installed:
   ```bash
   sudo yum install quota
   ```

2. Edit `/etc/fstab` and enable quotas on `/home`:
   ```bash
   /dev/sda2  /home  ext4  defaults,usrquota  0  2
   ```

3. Remount `/home`:
   ```bash
   sudo mount -o remount /home
   ```

4. Create quota files and enable quotas:
   ```bash
   sudo quotacheck -cug /home
   sudo quotaon /home
   ```

5. Set the quotas for user `alice`:
   ```bash
   sudo edquota alice
   ```
   In the editor:
   ```
   Disk quotas for user alice (uid 1002):
     Filesystem     blocks   soft   hard    inodes   soft   hard
     /dev/sda2        0       500000  600000   0     200     250
   ```
   
   blocks: This is the disk space in 1K blocks (500000 = 500MB, 600000 = 600MB).
   
   soft: The user will receive warnings when they exceed this limit.
   
   hard: The user cannot exceed this limit.
   
7. Verify quota settings:
   ```bash
   quota alice
   ```

---

### Important Commands for Quotas:

- **Modify fstab**: `vim /etc/fstab`
- **Remount filesystem**: `mount -o remount /home`
- **Create quota database**: `quotacheck -cum /home`
- **Turn on quotas**: `quotaon /home`
- **Edit user quotas**: `edquota -u [username]`
- **Edit group quotas**: `edquota -g [groupname]`
- **Check quota for user**: `quota -u [username]`
- **Check quota for group**: `quota -g [groupname]`
- **Verify quota status**: `quotaon -p /home`

---









### 3. **Monitor and Adjust Quotas Based on Usage**
   - **Question**: 
     The user `bob` on the `/srv` partition has been using 80% of their quota. Their current limits are set to a **soft limit of 500MB** and a **hard limit of 550MB**. Increase `bob`'s soft limit to **800MB** and the hard limit to **900MB** without affecting the quota settings of other users. Ensure the change is persistent.

   - **Solution**:
     1. Open the quota configuration for `bob` using `edquota -u bob`.
     2. Update the soft limit to `800000` and the hard limit to `900000` blocks (each block is 1KB).
     3. Save the changes and verify with `quota -u bob`.
     4. Ensure the quota persists after reboot by checking `/etc/fstab` and rebooting.

---

### 4. **Setting Quotas with Inode Limits**
   - **Question**: 
     On the `/opt` partition, configure **user quotas** for `carol`. Set a **soft limit of 1000 inodes** (files) and a **hard limit of 1500 inodes**. Ensure that these quotas persist and are effective across reboots.

   - **Solution**:
     1. Modify `/etc/fstab` to include `usrquota` for `/opt`.
     2. Remount the `/opt` partition using `mount -o remount /opt`.
     3. Initialize the quota database using `quotacheck -cum /opt`.
     4. Turn on quotas using `quotaon /opt`.
     5. Use `edquota -u carol` to set the inode soft limit to `1000` and hard limit to `1500`.
     6. Verify the inode limits with `quota -u carol`.
     7. Reboot to verify persistence.

---

### 5. **Check and Report Quota Usage for Multiple Users**
   - **Question**:
     The system administrator asks you to generate a report on the disk usage of users `dave`, `emma`, and `frank` on the `/data` partition, where quotas have already been set up. Check their disk usage and verify their quota limits.

   - **Solution**:
     1. Use the `repquota /data` command to generate a full report for all users on the `/data` partition.
     2. Filter the output to show quota details specifically for `dave`, `emma`, and `frank` using `quota -u dave emma frank`.
     3. Review the soft and hard limits, as well as current usage for each user.

---

### 6. **Grace Period Configuration for Soft Limit Breaches**
   - **Question**:
     On the `/projects` partition, users `developer1` and `developer2` have been reaching their **soft limit** frequently. Extend the grace period for the soft limit to **7 days**. Ensure this setting is effective across reboots.

   - **Solution**:
     1. Use the `edquota -t` command to edit the global grace period settings.
     2. Set the grace period for block limits and inode limits to `7 days`.
     3. Verify the grace period using `quota -u developer1 developer2`.
     4. Ensure the quota is persistent by checking `/etc/fstab` for quota enablement.

---

### 7. **Quota Warnings and Soft Limit Exceedance**
   - **Question**:
     The user `john` on the `/home` partition has exceeded their **soft limit** of **400MB** but is still below their **hard limit** of **450MB**. Verify that they are receiving quota warnings and explain how the system behaves when the soft limit is exceeded but the hard limit is not yet reached.

   - **Solution**:
     1. Use `quota -u john` to check the current disk usage and the limits for user `john`.
     2. Explain that when the soft limit is exceeded, the user receives a warning and has a certain **grace period** (by default, 7 days) to reduce usage below the soft limit. If they don’t, the hard limit is enforced, and no more data can be written beyond that point.

---

### 8. **Removing Quotas for a User**
   - **Question**:
     A user `jane` no longer requires quotas on the `/var` partition. Remove all quotas for `jane` but keep quotas enabled for other users on the same partition.

   - **Solution**:
     1. Open the quota file for user `jane` using `edquota -u jane`.
     2. Set both the soft and hard limits to `0`, effectively removing any quotas.
     3. Verify using `quota -u jane`.
     4. Ensure that other users still have their quotas intact by using `repquota /var`.

---

### 9. **Troubleshoot Quota-Related Issues**
   - **Question**:
     Users on the `/data` partition report that their quotas are not being enforced. Quotas were previously set up, but after a recent reboot, no quotas are applied. Diagnose and resolve the issue, ensuring that quotas are automatically applied on reboot.

   - **Solution**:
     1. Check if quotas are enabled by running `quotaon -p /data`.
     2. If quotas are disabled, check the `/etc/fstab` file for the `/data` partition and verify that `usrquota` or `grpquota` is still enabled.
     3. If quotas are missing from `/etc/fstab`, re-add them, remount the partition, and re-enable quotas using `quotaon /data`.
     4. Verify the quota files and ensure `quotacheck` is run if needed.

---

These modified questions are designed to help you understand quota management in RHEL 9, while ensuring you get a variety of tasks to practice, from enabling quotas to troubleshooting them. In the **RHCSA exam**, it's essential to know how to apply, verify, and maintain quota settings across reboots, and these exercises cover all those aspects.












________________________________________________________________________________________________




install quota

```bash
dnf install quota
```

________________________________________________________________________________________________

# Enable Quota:

________________________________________________________________________________________________


## `edquota --user`

### edit the quata of a user

```bash
edquota --user hamid
```


blocks: the allocated size: 102400 --> 100M

1 block = 1 KiloByte

soft limits / hard limits

0 = no limits


each directory/file = 1 inode

to limite the number of files/directories, use inode

________________________________________________________________________________________________


## `edquota --user hamid`

### to Edit the quata of a user


```bash
edquota --user hamid
```


________________________________________________________________________________________________


## `quota --user hamid`

### to Verify the quata of a user


```bash
quota --user hamid
```

grace time : when we pass the soft limit, we have time for deleting the extra data / or we will de blocked from any additional change


________________________________________________________________________________________________


## `quota --edit-period`

### to Edit the quata of a group

```bash
quota --edit-period
```

________________________________________________________________________________________________


## `edquota --group`

edit the quota of a group:

```bash
edquota --group adm
```

________________________________________________________________________________________________


## `quota --group`

### to Verify the quata of a user

```bash
quota --group adm
```
________________________________________________________________________________________________
________________________________________________________________________________________________

## `xfs_quota -x -c`

Edit disk quotas for the user called john. Set a soft limit of 100 megabytes and hard limit of 500 megabytes on /mnt partition.

```bash
[bob@centos-host ~]$ sudo xfs_quota -x -c 'limit bsoft=100m bhard=500m john' /mnt/
```

