

install docker

```bash
[bob@centos-host ~]$ dnf install docker
```

________________________________________________________________________________________________


search for an image

```bash
[bob@centos-host ~]$ docker search nginx

NAME                                              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
nginx                                             Official build of Nginx.                        18530     [OK]       
```

________________________________________________________________________________________________


Docker pull

```bash
[bob@centos-host ~]$ docker pull nginx
```

OR

```bash
[bob@centos-host ~]$ docker pull nginx:1.20.2
```

________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ docker images

REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
mariadb      latest    4af0c16be4b1   11 days ago     403MB
nginx        latest    448a08f1d2f9   2 weeks ago     142MB
nginx        1.20.2    0584b370e957   12 months ago   141MB
```

________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ docker rmi nginx:1.20.2
```

OR

```bash
[bob@centos-host ~]$ docker rmi 0584b370e957
```


________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ docker run -d nginx

3d85773a48f6e40888847811a262d47824954066d141198a90c40cf357ca7859
```

________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ docker ps

CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS     NAMES
3d85773a48f6   nginx     "/docker-entrypoint.…"   4 seconds ago   Up 2 seconds   80/tcp    zealous_hugle
```

________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ docker stop zealous_hugle
```

________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED              STATUS                      PORTS     NAMES
3d85773a48f6   nginx     "/docker-entrypoint.…"   About a minute ago   Exited (0) 11 seconds ago             zealous_hugle
```

________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ docker rm zealous_hugle
zealous_hugle
```

________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ docker run -d -p 800:80 --name my-web-server nginx
```

________________________________________________________________________________________________
