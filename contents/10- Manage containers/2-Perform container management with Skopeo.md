

## Skopeo is a tool for manipulating, inspecting, signing, and transferring container images and image repositories on Linux® systems, Windows and MacOS.

## Like Podman and Buildah, Skopeo is an open source community-driven project that does not require running a container daemon.


```bash
[bob@centos-host ~]$ sudo yum install skopeo -y

[bob@centos-host ~]$ skopeo login registry.redhat.io

Username: myrhusername
Password: ***********
```

________________________________________________________________________________________________


## `skopeo inspect docker://`

```bash
skopeo inspect docker://nginx:latest
```

```bash
[bob@centos-host ~]$ sudo skopeo inspect docker://registry.fedoraproject.org/fedora:latest

{
    "Name": "registry.fedoraproject.org/fedora",
    "Digest": "sha256:490a2eb8c9ae75eb4f1cef7cd6bcd73c3fcc00e1a4822d3be592ff917b1353cf",
    "RepoTags": [
        "34-aarch64",
        "34-ppc64le",
        "34-x86_64",
        "34",
        "34-s390x",
        "34-armhfp",
        "33-armhfp",
        "32-armhfp",
        "35-aarch64",
        "35-ppc64le",
        "35-s390x",
        "35-x86_64",
        "35",
        "35-armhfp",
        "36-aarch64",
        "36-armhfp",
        "36-ppc64le",
        "36-s390x",
        "36-x86_64",
        "30-aarch64",
        "30-ppc64le",
        "30-s390x",
        "30-x86_64",
        "30",
        "latest",
        "rawhide",
        "30-armhfp",
        "36",
        "31-aarch64",
        "31-x86_64",
        "31",
        "31-armhfp",
        "31-s390x",
        "31-ppc64le",
        "32-aarch64",
        "32-ppc64le",
        "32-s390x",
        "32-x86_64",
        "32",
        "33-aarch64",
        "33-ppc64le",
        "33-s390x",
        "33-x86_64",
        "33",
        "37-aarch64",
        "37-ppc64le",
        "37-s390x",
        "37-x86_64",
        "37",
        "38-aarch64",
        "38-ppc64le",
        "38-s390x",
        "38-x86_64",
        "38",
        "39-aarch64",
        "39-s390x",
        "39-x86_64",
        "39",
        "39-ppc64le",
        "40-aarch64",
        "40-ppc64le",
        "40-s390x",
        "40-x86_64",
        "40"
    ],
    "Created": "2023-11-28T07:50:13Z",
    "DockerVersion": "1.10.1",
    "Labels": {
        "license": "MIT",
        "name": "fedora",
        "vendor": "Fedora Project",
        "version": "39"
    },
    "Architecture": "amd64",
    "Os": "linux",
    "Layers": [
        "sha256:718a00fe32127ad01ddab9fc4b7c968ab2679c92c6385ac6865ae6e2523275e4"
    ],
    "Env": [
        "DISTTAG=f39container",
        "FGC=f39",
        "container=oci"
    ]
}
```

________________________________________________________________________________________________



## `skopeo inspect --config`


```bash
skopeo inspect --config docker://nginx:latest | jq
```

________________________________________________________________________________________________


## `skopeo copy`

### copy an image from `one registry` to `another registry`

```bash
skopeo copy docker://internal.registry/myimage:latest docker://production.registry/myimage:v1.0
```

________________________________________________________________________________________________

## `skopeo delete`


### Delete an image from out local machine

```bash
skopeo delete docker://localhost:5000/myimage:v1.0
```

________________________________________________________________________________________________


## `skopeo sync --src docker --dest dir`

### Synchronizing from remot registry to a local directory


```bash
[bob@centos-host ~]$ sudo skopeo sync --src docker --dest dir registry.fedoraproject.org/fedora:latest /home/bob/fedora

INFO[0000] Tag presence check                            imagename="registry.fedoraproject.org/fedora:latest" tagged=true
INFO[0000] Copying image ref 1/1                         from="docker://registry.fedoraproject.org/fedora:latest" to="dir:/home/bob/fedora/fedora:latest"
Getting image source signatures
Copying blob 718a00fe3212 done  
Copying config 368a084ba1 done  
Writing manifest to image destination
Storing signatures
INFO[0001] Synced 1 images from 1 sources               
[bob@centos-host ~]$ ls /home/bob/fedora
fedora:latest
```

```bash              
[bob@centos-host ~]$ ls /home/bob/fedora

fedora:latest
```

________________________________________________________________________________________________




```bash
man skopeo-copy
```

________________________________________________________________________________________________
