
### Create a Containerfile:



```Dockerfile
FROM registry.redhat.io/ubi8/ubi

RUN dnf -y install httpd

EXPOSE 80

CMD ["/usr/sbin/httpd", "-D", "FOREGROUND"]
```


```bash
FROM registry.redhat.io/ubi9/ubi-minimal:9.1.0

RUN microdnf install -y nginx

RUN rm -r /usr/share/nginx/html/*

COPY index.html /usr/share/nginx/html/

COPY startup.sh /

EXPOSE 80

CMD /startup.sh
```


________________________________________________________________________________________________





### build the image:

```bash
podman build . -t mynginx:test
```


________________________________________________________________________________________________
