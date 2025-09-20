## Instal·lació d'un servidor Nginx en Linux Ami 2023 (Fedora)

```
dnf update -y
dnf install nginx -y
systemctl start nginx
systemctl enable nginx
systemctl status nginx
cd /usr/share/nginx/html
rm index.html
wget https://sanvalero-static-webs.s3.eu-west-1.amazonaws.com/breakout.zip
unzip breakout.zip
```

## Instal·lació d'un servidor Apache en Ubuntu 

```
sudo apt update -y
sudo apt install -y apache2
sudo systemctl start apache2
sudo systemctl enable apache2
echo "<h1>Benvingut a la meua web d'Apache en Ubuntu en una EC2</h1>" | sudo tee /var/www/html/index.html
```



