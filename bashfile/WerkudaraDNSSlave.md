# INI BUAT SI WERKUDARA

```bash
echo nameserver 192.168.122.1 > /etc/resolv.conf
cat /etc/resolv.conf
apt-get update
apt-get install bind9 —y
service bind9 restart

# nano /etc/bind/named.conf.local
echo '
    zone "abimanyu.i09.com" {
        type slave;
	    masters { 10.63.2.2; };
        file "/etc/bind/jarkom/abimanyu.i09.com";
    };

    zone "baratayuda.abimanyu.i09.com" {
    type master;
    file "/etc/bind/delegasi/baratayuda.abimanyu.i09.com";
    }; ' > /etc/bind/named.conf.local

mkdir /etc/bind/delegasi
cp /etc/bind/db.local /etc/bind/delegasi/baratayuda.abimanyu.i09.com

echo ';
; BIND data file for local loopback interface
;
$TTL 604800
@           IN      SOA     baratayuda.abimanyu.i09.com    root.baratayuda.abimanyu.i09.com  (
                            2023101001          ; Serial
                                604800          ; Refresh
                                 86400          ; Retry
                               2419200          ; Expiry
                                604800 )        ; Negative Cache TTL
;
@           IN      NS      baratayuda.abimanyu.i09.com.
@           IN      A       10.63.3.3           ; IP AbimanyuWebServer
www         IN      CNAME   baratayuda.abimanyu.i09.com.
rjp         IN      A       10.63.3.3           ; IP AbimanyuWebServer
www.rjp     IN      CNAME   rjp.baratayuda.abimanyu.i09.com.
@           IN      AAAA    ::1' > /etc/bind/delegasi/baratayuda.abimanyu.i09.com

service bind9 restart
```