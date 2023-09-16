

`module packages`:    group `multiple version` of `software packages` together

`profile`: a `set of packages` meant to be installed together for a specific purpose or usecase 

`module streams`: -->  `Active` / `InActive`

*** Only one version of any module stream can be enabled at any time ( only one version of a package can be installed ad any time )

`Modules` and their dependencies are handled by `yum`


________________________________________________________________________________________________


## `yum module list`

to see available modules on the system

```bash
[bob@centos-host ~]$ yum module list

Name                 Stream          Profiles Summary 
httpd                2.4 [d]         common [ Apache HTTP Server                         
                                     d], deve 
                                     l, minim 
                                     al       
idm                  DL1             adtrust, The Red Hat Enterprise Linux Identity Manag
                                      client, ement system module
                                      common  
                                     [d], dns 
                                     , server 
idm                  client [d]      common [ RHEL IdM long term support client module   
                                     d]       

nodejs               12              common [ Javascript runtime                         
                                     d], deve 
                                     lopment, 
                                      minimal 
                                     , s2i    
```

________________________________________________________________________________________________


## `yum module list nodejs`

to see info of a specific module ( ex: nodejs )

stream = version

[d] = default


```bash
[bob@centos-host ~]$ yum module list nodejs

Last metadata expiration check: 0:02:32 ago on Sun May 21 15:16:44 2023.
CentOS Stream 8 - AppStream
Name        Stream      Profiles                                   Summary               
nodejs      10 [d]      common [d], development, minimal, s2i      Javascript runtime    
nodejs      12          common [d], development, minimal, s2i      Javascript runtime    
nodejs      14          common [d], development, minimal, s2i      Javascript runtime    
nodejs      16          common [d], development, minimal, s2i      Javascript runtime    
nodejs      18          common, development, minimal, s2i          Javascript runtime    
```


the default version (nodejs 10) will be installed by this command:

```bash
[bob@centos-host ~]$ yum module install nodejs
```

________________________________________________________________________________________________


## `yum module install`

when we install a module, it will install the default version, but you can specify the exact version and profile

```bash
[bob@centos-host ~]$ sudo yum module install nodejs:14/development
```

________________________________________________________________________________________________




## `yum module list --installed`



```bash
[bob@centos-host ~]$ yum module list --installed nodejs

Name       Stream     Profiles                                      Summary              
nodejs     14 [e]     common [d], development [i], minimal, s2i     Javascript runtime   

Hint: [d]efault, [e]nabled, [x]disabled, [i]nstalled
```

________________________________________________________________________________________________


## `yum module reset`

to reset module to the `default version` (and `common profile`)

```bash
[bob@centos-host ~]$ sudo yum module reset nodejs
```

________________________________________________________________________________________________
