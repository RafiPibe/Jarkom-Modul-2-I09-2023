# INI BUAT SI YUDHISTIRA

```bash
echo nameserver 192.168.122.1 > /etc/resolv.conf
cat /etc/resolv.conf
apt-get update
apt-get install bind9 —y
service bind9 restart

mkdir /etc/bind/jarkom
# nano /etc/bind/named.conf.local
echo '
    zone "arjuna.i09.com" {
 	    type master;
 	    file "/etc/bind/jarkom/arjuna.i09.com";
    };

    zone "abimanyu.i09.com" {
        type master;
	    also-notify { 10.63.2.3; };
	    allow-transfer { 10.63.2.3; };
        file "/etc/bind/jarkom/abimanyu.i09.com";
    };

    zone "2.63.10.in-addr.arpa" {
    type master;
    file "/etc/bind/jarkom/2.63.10.in-addr.arpa";
    }; ' > /etc/bind/named.conf.local

# nano /etc/bind/named.conf.options
echo 'options {
    directory "/var/cache/bind";
    //dnssec-validation auto;
    allow-query{any;};

    auth-nxdomain no;   # conform to RFC1035
    listen-on-v6 { any; };
};' > /etc/bind/named.conf.options

cp /etc/bind/db.local /etc/bind/jarkom/arjuna.i09.com
# nano /etc/bind/jarkom/arjuna.i09.com
echo ';
; BIND data file for local loopback interface
;
$TTL 604800
@           IN      SOA     arjuna.i09.com      root.arjuna.i09.com  (
                            2023101001          ; Serial
                                604800          ; Refresh
                                 86400          ; Retry
                               2419200          ; Expiry
                                604800 )        ; Negative Cache TTL
;
@           IN      NS      arjuna.i09.com.
@           IN      A       10.63.2.2           ; IP YudhistiraDNSMaster
www         IN      CNAME   arjuna.i09.com.
@           IN      AAAA    ::1' > /etc/bind/jarkom/arjuna.i09.com

cp /etc/bind/db.local /etc/bind/jarkom/abimanyu.i09.com
# nano /etc/bind/jarkom/abimanyu.i09.com
echo ';
; BIND data file for local loopback interface
;
$TTL 604800
@           IN      SOA     abimanyu.i09.com    root.abimanyu.i09.com  (
                            2023101001          ; Serial
                                604800          ; Refresh
                                 86400          ; Retry
                               2419200          ; Expiry
                                604800 )        ; Negative Cache TTL
;
@           IN      NS      abimanyu.i09.com.
@           IN      A       10.63.2.2           ; IP YudhistiraDNSMaster
www         IN      CNAME   abimanyu.i09.com.
parikesit   IN      A       10.63.3.3           ; IP AbimanyuWebServer
ns1         IN      A       10.63.2.3           ; IP WerkudaraDNSSlave
baratayuda  IN      NS      ns1
@           IN      AAAA    ::1' > /etc/bind/jarkom/abimanyu.i09.com

cp /etc/bind/db.local /etc/bind/jarkom/2.63.10.in-addr.arpa
# nano /etc/bind/jarkom/2.63.10.in-addr.arpa
echo ';
; BIND data file for local loopback interface
;
$TTL 604800
@                       IN      SOA     abimanyu.i09.com    root.abimanyu.i09.com  (
                            2023101001          ; Serial
                                604800          ; Refresh
                                 86400          ; Retry
                               2419200          ; Expiry
                                604800 )        ; Negative Cache TTL
;
2.63.10.in-addr.arpa.   IN      NS      abimanyu.i09.com.
@                       IN      PTR     abimanyu.i09.com.' > /etc/bind/jarkom/2.63.10.in-addr.arpa

service bind9 restart
```