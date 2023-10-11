# INI BUAT SI WEB SERVER

```bash
echo nameserver 192.168.122.1 > /etc/resolv.conf
cat /etc/resolv.conf

apt-get update && apt install nginx php php-fpm -y
php -v
mkdir /var/www/jarkom
```