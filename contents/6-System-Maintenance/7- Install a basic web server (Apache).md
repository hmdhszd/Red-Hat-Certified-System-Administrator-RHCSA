# Install Apache web server

1- Install

```bash
yum install httpd -y
```
2- create the first page

```bash
echo "Hello from Apache" > /var/www/html/index.html

curl http://localhost
```

3- open firewall

```bash
firewall-cmd --zone=public --add-service=http
firewall-cmd --zone=public --add-service=http --permanent
firewall-cmd --zone=public --add-service=https
firewall-cmd --zone=public --add-service=https --permanent
```
