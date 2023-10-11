# INI BUAT SI ARJUNA

```bash
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install dnsutils -y
apt-get install bind9 nginx -y

#
echo nameserver 10.63.2.2 > /etc/resolv.conf
echo nameserver 10.63.2.3 >> /etc/resolv.conf
cat /etc/resolv.conf
```