

install docker

```bash
[bob@centos-host ~]$ dnf install docker
```

________________________________________________________________________________________________


search for an image

```bash
[bob@centos-host ~]$ podman search nginx

INDEX              NAME                                                          DESCRIPTION                                      STARS       OFFICIAL    AUTOMATED
docker.io          docker.io/library/nginx                                       Official build of Nginx.                         19397       [OK]        
docker.io          docker.io/library/unit                                        Official build of NGINX Unit: Universal Web ...  19          [OK]        
docker.io          docker.io/nginxproxy/acme-companion                           Automated ACME SSL certificate generation fo...  127                     
docker.io          docker.io/bitnami/nginx                                       Bitnami nginx Docker Image                       180                     [OK]
docker.io          docker.io/bitnami/nginx-ingress-controller                    Bitnami Docker Image for NGINX Ingress Contr...  32                      [OK]
```

________________________________________________________________________________________________


podman pull

```bash
[bob@centos-host ~]$ podman pull nginx

✔ docker.io/library/nginx:latest
Trying to pull docker.io/library/nginx:latest...
Getting image source signatures
Copying blob 8c37d2ff6efa done  
Copying blob 336ba1f05c3e done  
Copying blob 5e99d351b073 done  
Copying blob 51d6357098de done  
Copying blob 782f1ecce57d done  
Copying blob af107e978371 done  
Copying blob 7b73345df136 done  
Copying config d453dd892d done  
Writing manifest to image destination
Storing signatures
d453dd892d9357f3559b967478ae9cbc417b52de66b53142f6c16c8a275486b9
```

OR

```bash
[bob@centos-host ~]$ podman pull nginx:1.20.2
```

________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ podman images

REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
mariadb      latest    4af0c16be4b1   11 days ago     403MB
nginx        latest    448a08f1d2f9   2 weeks ago     142MB
nginx        1.20.2    0584b370e957   12 months ago   141MB
```

________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ podman rmi nginx:1.20.2
```

OR

```bash
[bob@centos-host ~]$ podman rmi 0584b370e957
```


________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ podman run -d nginx

3d85773a48f6e40888847811a262d47824954066d141198a90c40cf357ca7859
```

________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ podman ps

CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS     NAMES
3d85773a48f6   nginx     "/docker-entrypoint.…"   4 seconds ago   Up 2 seconds   80/tcp    zealous_hugle
```

________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ podman stop zealous_hugle
```

________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ podman ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED              STATUS                      PORTS     NAMES
3d85773a48f6   nginx     "/docker-entrypoint.…"   About a minute ago   Exited (0) 11 seconds ago             zealous_hugle
```

________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ podman rm zealous_hugle
zealous_hugle
```

________________________________________________________________________________________________




```bash
[bob@centos-host ~]$ sudo podman run -d -p 1234:80 --name my-web-server nginx
```



```bash
[bob@centos-host ~]$ sudo podman ps 
CONTAINER ID  IMAGE                           COMMAND               CREATED             STATUS                 PORTS                 NAMES
66a7ccb8bbf9  docker.io/library/nginx:latest  nginx -g daemon o...  About a minute ago  Up About a minute ago  0.0.0.0:1234->80/tcp  my-web-server
```



________________________________________________________________________________________________



Use podman to search for the official Redis container.

```bash
podman search redis --filter=is-official
```
________________________________________________________________________________________________

Add the tag "myredis" to the "docker.io/library/redis" image.

```bash
podman tag docker.io/library/redis myredis
```
________________________________________________________________________________________________


Set the SELinux Boolean value of "container_manage_cgroup" to "on" and ensure persistence across reboots.

```bash
setsebool -P container_manage_cgroup on
```



________________________________________________________________________________________________






