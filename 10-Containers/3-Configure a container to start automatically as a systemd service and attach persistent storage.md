

change the module stream for the container-tools to 3.0 in order to update the podman

```bash
sudo yum module reset container-tools
```


```bash
sudo yum module install container-tools:3.0 
```

________________________________________________________________________________________________


running a container, at boot time, as a rootless systemd service

(container runs as a service under a regular user rather that root user)

FIRST, we need to create a directory in the user's home directory in order to house the service file:

```bash
mkdir -p ~/.config/systemd/user
mkdir -p ~/container_storage
```


________________________________________________________________________________________________




```bash
echo "This is a Test web Page" > ~/container_storage/kodekloud.html
```

________________________________________________________________________________________________

Second, we run the container:

```bash
podman -d --name container-service -p 1025:8080 -v ~/container_storage:/var/www/html:Z registry.access.redhat.com/rhscl/httpd-24-rhel7
```

________________________________________________________________________________________________




```bash
cd ~/.config/systemd/user

podman generate systemd --name container_service --files --new
```




```bash
cat ~/.config/systemd/user
```

________________________________________________________________________________________________

now we can kill and remove the container,

```bash
podman kill container_service
podman rm container_service
```

________________________________________________________________________________________________


Next, in order to make sure that the user can lunch the systemd services, we need to enable a login setting for a user called `linger`:


```bash
loginctl enable-linger
```

```bash
systemctl --user daemon-reload
systemctl --user enable --now container-container_service.service
```

________________________________________________________________________________________________


now, we will reboot the system, and the container will be run at startup

```bash
reboot
```


```bash
podman ps -a
```




```bash
curl 127.0.0.1:1025/index.html
```


________________________________________________________________________________________________
