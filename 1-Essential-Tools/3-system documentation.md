
## `--help`

The help command provides information on built-in commands

```bash
 [bob@centos-host ~]$ ls --help
 
Usage: ls [OPTION]... [FILE]...
List information about the FILEs (the current directory by default).
Sort entries alphabetically if none of -cftuvSUX nor --sort is specified.

Mandatory arguments to long options are mandatory for short options too.
  -a, --all                  do not ignore entries starting with .
  -A, --almost-all           do not list implied . and ..
      --author               with -l, print the author of each file
  -b, --escape               print C-style escapes for nongraphic characters
      --block-size=SIZE      with -l, scale sizes by SIZE when printing them;
                               e.g., '--block-size=M'; see SIZE format below
  -B, --ignore-backups       do not list implied entries ending with ~
  -c                         with -lt: sort by, and show, ctime (time of last
                               modification of file status information);
                               with -l: show ctime and sort by name;
```

________________________________________________________________________________________________

install man-pages:

```bash
[bob@centos-host ~]$ yum install man-pages
```

________________________________________________________________________________________________


## `man`

man command in Linux is used to display the user manual of any command that we can run on the terminal. 

```bash
[bob@centos-host ~]$ man ls
```

________________________________________________________________________________________________

## `man man`

some commands have more than one manual pages

```bash
[bob@centos-host ~]$ man man

       The table below shows the section numbers of the manual followed by  the  types
       of pages they contain.

       1   Executable programs or shell commands
       2   System calls (functions provided by the kernel)
       3   Library calls (functions within program libraries)
       4   Special files (usually found in /dev)
       5   File formats and conventions eg /etc/passwd
       6   Games
       7   Miscellaneous  (including  macro  packages  and  conventions), e.g. man(7),
           groff(7)
       8   System administration commands (usually only for root)
       9   Kernel routines [Non standard]

```

we can call each of them seperately:


```bash
[bob@centos-host ~]$ man 1 printf
[bob@centos-host ~]$ man 3 printf
```

________________________________________________________________________________________________

## `mandb`

for search a word in the man page of all commands, in order to find out the command, use apropos

but before that we should update the database:

```bash
[bob@centos-host ~]$ sudo mandb

Purging old database entries in /usr/share/man/overrides...
Processing manual pages under /usr/share/man/overrides...
Purging old database entries in /usr/share/man...
Processing manual pages under /usr/share/man...
Purging old database entries in /usr/share/man/overrides...
Processing manual pages under /usr/share/man/overrides...
Purging old database entries in /usr/share/man/ru...
Processing manual pages under /usr/share/man/ru...
Purging old database entries in /usr/local/share/man...
Processing manual pages under /usr/local/share/man...
0 man subdirectories contained newer manual pages.
0 manual pages were added.
0 stray cats were added.
0 old database entries were purged.
```



## `apropos`


```bash
[bob@centos-host ~]$ apropos directory

access (3p)          - determine accessibility of a file relative to directory file de...
alphasort (3)        - scan a directory for matching entries
alphasort (3p)       - scan a directory
basename (1p)        - return non-directory portion of a pathname
bindtextdomain (3)   - set directory containing message catalogs
cd (1p)              - change the working directory
chdir (2)            - change working directory
chdir (3p)           - change working directory
chmod (3p)           - change mode of a file relative to directory file descriptor
chown (3p)           - change owner and group of a file relative to directory file des...
chroot (2)           - change root directory
closedir (3)         - close a directory
.
.
.
```

________________________________________________________________________________________________

## `info`

Command info display information in the document format. 

```bash
[bob@centos-host ~]$ info ls
```

________________________________________________________________________________________________

## `/usr/share/doc`

documents of each software package:

```bash
[bob@centos-host ~]$ ls /usr/share/doc

adobe-mappings-cmap                     libXmu               perl-Data-Dumper
adobe-mappings-pdf                      libXpm               perl-Digest
alsa-lib                                libXrandr            perl-Digest-MD5
annobin-plugin                          libXrender           perl-Encode
asciidoc                                libXt                perl-Error
atk                                     libXtst              perl-Exporter
augeas-libs                             libXv                perl-File-Path
autoconf                                libXxf86misc         perl-File-Temp
autogen-libopts                         libXxf86vm           perl-Getopt-Long
automake                                libaio               perl-HTTP-Tiny
avahi-libs                              libarchive           perl-IO-Socket-IP
binutils                                libatomic_ops        perl-IO-Socket-SSL
```

________________________________________________________________________________________________
