# Password reset on RHEL 9

## Method 1:


1. `Reboot` the system.

2. On the GRUB 2 boot screen, use the `down` `arrow key` to select rescue mode.

3. `Press` the `E` key to interrupt the boot process.

4. Go to the end of the line starting with linux by pressing `Ctrl+E`.

5. Add `rd.break` to the end of the line.

6. Press `Ctrl+X` to start the system with the changed parameters.

7. To remount the file system as writable, run: `# mount -o remount,rw /sysroot`

8. Enter the chroot environment: `# chroot /sysroot`

9. Reset the root password: `# passwd`

10. Enable SELinux relabeling: `# touch /.autorelabel`

11. Press `Ctrl+D` twice to exit the chroot environment and maintenance mode.



OR

## Method 2:

1. `Reboot` the system.

2. On the GRUB 2 boot screen, use the `down` `arrow key` to select rescue mode.

3. `Press` the `E` key to interrupt the boot process.

4. Go to the end of the line starting with linux by pressing `Ctrl+E`.

5. Add `init=/bin/bash` to the end of the line, and change `ro` to `rw`.

6. Press `Ctrl+X` to start the system with the changed parameters.

7. Reset the root password: `# passwd`

8. Enable SELinux relabeling: `# touch /.autorelabel`

9. Initialize the system: `# exec /sbin/init`

