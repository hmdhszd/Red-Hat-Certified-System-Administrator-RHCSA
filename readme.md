# Red Hat Certified System Administrator (RHCSA) - EX200

Welcome to the **RHCSA Exam Preparation Guide**!

This document will help you organize and keep track of the key topics needed to prepare for the Red Hat Certified System Administrator (RHCSA) exam.

You may find the exam objectives in the [RedHat website](https://www.redhat.com/en/services/training/ex200-red-hat-certified-system-administrator-rhcsa-exam?section=objectives).

1. [Understand and use essential tools](#1-understand-and-use-essential-tools)
2. [Create simple shell scripts](#2-create-simple-shell-scripts)
3. [Operate running systems](#3-operate-running-systems)
4. [Configure local storage](#4-configure-local-storage)
5. [Create and configure file systems](#5-create-and-configure-file-systems)
6. [Deploy, configure, and maintain systems](#6-deploy-configure-and-maintain-systems)
7. [Manage basic networking](#7-manage-basic-networking)
8. [Manage users and groups](#8-manage-users-and-groups)
9. [Manage security](#9-manage-security)
10. [Manage containers](#10-manage-containers)

---

## 1. Understand and use essential tools

| Subtopic                                                     | Link       |
|---------------------------------------------------------------|------------|
| Access a shell prompt and issue commands with correct syntax <br> Access remote systems using SSH | - [login to server](https://github.com/hmdhszd/Red-Hat-Certified-System-Administrator-RHCSA/blob/main/1-Essential-Tools/1-login%20to%20server.md)  |
| Use input-output redirection (>, >>, \|, 2>, etc.)             | - [input-output redirection](https://github.com/hmdhszd/Red-Hat-Certified-System-Administrator-RHCSA/blob/main/1-Essential-Tools/15-input-output%20redirection.md) |
| Use grep and regular expressions to analyze text              | - [grep](https://github.com/hmdhszd/Red-Hat-Certified-System-Administrator-RHCSA/blob/main/1-Essential-Tools/10-grep.md)  <br> <br> - [regex](https://github.com/hmdhszd/Red-Hat-Certified-System-Administrator-RHCSA/blob/main/1-Essential-Tools/11-regex.md)  |
| Log in and switch users in multiuser targets                  | - [Log in and switch users in multiuser targets](https://github.com/hmdhszd/Red-Hat-Certified-System-Administrator-RHCSA/blob/main/1-Essential-Tools/2-Log%20in%20and%20switch%20users%20in%20multiuser%20targets.md)  |
| Archive, compress, unpack, and uncompress files using tar, gzip, and bzip2 | - [Archive, backup, compress, unpack, and uncompress files](https://github.com/hmdhszd/Red-Hat-Certified-System-Administrator-RHCSA/blob/main/1-Essential-Tools/12-Archive%2C%20backup%2C%20compress%2C%20unpack%2C%20and%20uncompress%20files.md)  <br>  <br>  - [Compress and Uncompress files](https://github.com/hmdhszd/Red-Hat-Certified-System-Administrator-RHCSA/blob/main/1-Essential-Tools/13-Compress%20and%20Uncompress%20files.md) <br>  <br>  - [Archive, compress, unpack, and uncompress files using star](https://github.com/hmdhszd/Red-Hat-Certified-System-Administrator-RHCSA/blob/main/1-Essential-Tools/14-Archive%2C%20compress%2C%20unpack%2C%20and%20uncompress%20files%20using%20star.md) |
| Create and edit text files  <br>  Create, delete, copy, and move files and directories                                   | - [files and directories](https://github.com/hmdhszd/Red-Hat-Certified-System-Administrator-RHCSA/blob/main/1-Essential-Tools/4-files%20and%20directories.md)  <br> <br> - [Compare and manipulate file content](https://github.com/hmdhszd/Red-Hat-Certified-System-Administrator-RHCSA/blob/main/1-Essential-Tools/9-Compare%20and%20manipulate%20file%20content.md) |
| Create hard and soft links                                    | - [hard link](https://github.com/hmdhszd/Red-Hat-Certified-System-Administrator-RHCSA/blob/main/1-Essential-Tools/5-hard%20link.md) <br> <br> - [soft link](https://github.com/hmdhszd/Red-Hat-Certified-System-Administrator-RHCSA/blob/main/1-Essential-Tools/6-soft%20link.md)  |
| List, set, and change standard ugo/rwx permissions            |  - [file permissions](https://github.com/hmdhszd/Red-Hat-Certified-System-Administrator-RHCSA/blob/main/1-Essential-Tools/7-file%20permissions.md) |
| Locate, read, and use system documentation                    | - [system documentation](https://github.com/hmdhszd/Red-Hat-Certified-System-Administrator-RHCSA/blob/main/1-Essential-Tools/3-system%20documentation.md) <br> <br> - [Search for files](https://github.com/hmdhszd/Red-Hat-Certified-System-Administrator-RHCSA/blob/main/1-Essential-Tools/8-Search%20for%20files.md)  |

---

## 2. Create simple shell scripts

| Subtopic                                                    | Link       |
|--------------------------------------------------------------|------------|
| Conditionally execute code (use of: if, test, [], etc.)  <br>  <br> Use looping constructs (for, etc.) to process input  <br>  <br> Process script inputs ($1, $2, etc.) <br>  <br> Process output of shell commands within a script     | [Shell Script 1](https://github.com/hmdhszd/Red-Hat-Certified-System-Administrator-RHCSA/blob/main/2-Shell-Scripts/1-Shell%20Script%201.md) <br>  <br>  [Shell Script 2](https://github.com/hmdhszd/Red-Hat-Certified-System-Administrator-RHCSA/blob/main/2-Shell-Scripts/2-Shell%20Script%202.md) |

---

## 3. Operate running systems

| Subtopic                                                    | Link       |
|--------------------------------------------------------------|------------|
| Boot, reboot, and shut down a system normally                | [Link]()   |
| Boot systems into different targets manually                 | [Link]()   |
| Interrupt the boot process to gain access to a system        | [Link]()   |
| Identify CPU/memory intensive processes and kill processes   | [Link]()   |
| Adjust process scheduling                                    | [Link]()   |
| Manage tuning profiles                                       | [Link]()   |
| Locate and interpret system log files and journals           | [Link]()   |
| Preserve system journals                                     | [Link]()   |
| Start, stop, and check the status of network services        | [Link]()   |
| Securely transfer files between systems                      | [Backup files - transfer files](https://github.com/hmdhszd/Red-Hat-Certified-System-Administrator-RHCSA/blob/main/1-Essential-Tools/16-Backup%20files%20-%20transfer%20files.md)   |

---

## 4. Configure local storage

| Subtopic                                                    | Link       |
|--------------------------------------------------------------|------------|
| List, create, delete partitions on MBR and GPT disks         | [Link]()   |
| Create and remove physical volumes                           | [Link]()   |
| Assign physical volumes to volume groups                     | [Link]()   |
| Create and delete logical volumes                            | [Link]()   |
| Configure systems to mount file systems at boot using UUID or label | [Link]()   |
| Add new partitions, logical volumes, and swap to a system non-destructively | [Link]()   |

---

## 5. Create and configure file systems

| Subtopic                                                    | Link       |
|--------------------------------------------------------------|------------|
| Create, mount, unmount, and use vfat, ext4, and xfs file systems | [Link]()   |
| Mount and unmount network file systems using NFS             | [Link]()   |
| Configure autofs                                             | [Link]()   |
| Extend existing logical volumes                              | [Link]()   |
| Create and configure set-GID directories for collaboration   | [Link]()   |
| Diagnose and correct file permission problems                | [Link]()   |

---

## 6. Deploy, configure, and maintain systems

| Subtopic                                                    | Link       |
|--------------------------------------------------------------|------------|
| Schedule tasks using at and cron                             | [Link]()   |
| Start and stop services and configure to start automatically at boot | [Link]()   |
| Configure systems to boot into a specific target automatically | [Link]()   |
| Configure time service clients                               | [Link]()   |
| Install and update software packages                         | [Link]()   |
| Modify the system bootloader                                 | [Link]()   |

---

## 7. Manage basic networking

| Subtopic                                                    | Link       |
|--------------------------------------------------------------|------------|
| Configure IPv4 and IPv6 addresses                            | [Link]()   |
| Configure hostname resolution                                | [Link]()   |
| Configure network services to start automatically at boot    | [Link]()   |
| Restrict network access using firewall-cmd/firewalld         | [Link]()   |

---

## 8. Manage users and groups

| Subtopic                                                    | Link       |
|--------------------------------------------------------------|------------|
| Create, delete, and modify local user accounts               | [Link]()   |
| Change passwords and adjust password aging for local accounts | [Link]()   |
| Create, delete, and modify local groups and memberships      | [Link]()   |
| Configure superuser access                                   | [Link]()   |

---

## 9. Manage security

| Subtopic                                                    | Link       |
|--------------------------------------------------------------|------------|
| Configure firewall settings using firewall-cmd/firewalld     | [Link]()   |
| Manage default file permissions                              | [Link]()   |
| Configure key-based authentication for SSH                   | [Link]()   |
| Set enforcing and permissive modes for SELinux               | [Link]()   |
| List and identify SELinux file and process context           | [Link]()   |
| Restore default file contexts                                | [Link]()   |
| Manage SELinux port labels                                   | [Link]()   |
| Use boolean settings to modify SELinux settings              | [Link]()   |
| Diagnose and address routine SELinux policy violations       | [Link]()   |

---

## 10. Manage containers

| Subtopic                                                    | Link       |
|--------------------------------------------------------------|------------|
| Find and retrieve container images from a remote registry    | [Link]()   |
| Inspect container images                                     | [Link]()   |
| Perform container management (podman, skopeo)                | [Link]()   |
| Run, start, stop, and list running containers                | [Link]()   |
| Run a service inside a container                             | [Link]()   |
| Configure a container to start automatically as a systemd service | [Link]()   |
| Attach persistent storage to a container                     | [Link]()   |
