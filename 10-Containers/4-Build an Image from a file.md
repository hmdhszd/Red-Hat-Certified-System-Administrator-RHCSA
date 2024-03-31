
### Create a Containerfile:

```bash
RUN microdnf install -y nginx

FROM registry.redhat.io/ubi9/ubi-minimal:9.1.0

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
