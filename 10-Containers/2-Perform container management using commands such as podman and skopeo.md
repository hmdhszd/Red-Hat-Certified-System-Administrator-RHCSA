

## Skopeo is a tool for manipulating, inspecting, signing, and transferring container images and image repositories on Linux® systems, Windows and MacOS.

## Like Podman and Buildah, Skopeo is an open source community-driven project that does not require running a container daemon.


```bash
[bob@centos-host ~]$ sudo yum install skopeo -y
```

________________________________________________________________________________________________




```bash
skopeo inspect docker://nginx:latest
```

________________________________________________________________________________________________




```bash
skopeo inspect --config docker://nginx:latest | jq
```

________________________________________________________________________________________________


### copy an image from a registry to another

```bash
skopeo copy docker://internal.registry/myimage:latest docker://production.registry/myimage:v1.0
```

________________________________________________________________________________________________


### Delete an image from out local machine

```bash
skopeo delete docker://localhost:5000/myimage:v1.0
```

________________________________________________________________________________________________


### Synchronizing from remot registry to a local directory


```bash
skopeo sync --src docker --dest dir registry.example.com/busybox /media/usb
```

________________________________________________________________________________________________




```bash
man skopeo-copy
```

________________________________________________________________________________________________
