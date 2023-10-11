<h1>Jarkom-Modul-2-I09-2023</h1>
<p>Praktikum Jaringan Komputer Modul 2 I09</p>
<p>Computer Network Practicum 2nd Module I09</p>

| Name                        | NRP        |
|-----------------------------|------------|
|Eric Azka Nugroho            | 5025211064 |
|Faraihan Rafi Adityawarman   | 5025211074 |

<h1>Scripting</h1>

for scripting, we use the command `nano /root/.bashrc` to start doing the bash scripting

YudhistiraDNSMaster
```bash
echo nameserver 192.168.122.1 > /etc/resolv.conf
cat /etc/resolv.conf
apt-get update
apt-get install bind9 —y
service bind9 restart
```
Connect to YudhistiraDNSMaster 
```bash
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install dnsutils -y

#
echo nameserver 10.63.2.2 > /etc/resolv.conf
echo nameserver 10.63.2.3 >> /etc/resolv.conf
cat /etc/resolv.conf
```


<h1>Problem 1</h1>
<p>Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni.</p>
<p align="center">
    <img src="https://media.discordapp.net/attachments/1153305482438660178/1161152416301973504/image.png?ex=65374275&is=6524cd75&hm=ede8ea79ef4f0a7db7a26fcda587c1a8adf7990536b9e8a735aa6455f65431c8&=&width=979&height=702" alt="Topology">
</p>
<p align="center">The Topology</p>

Router Network Configuration
```bash
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.63.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.63.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 10.63.3.1
	netmask 255.255.255.0
```
<h2>Switch1 Group</h2>

NakulaClient Network Configuration
```bash
auto eth0
iface eth0 inet static
	address 10.63.1.2
	netmask 255.255.255.0
	gateway 10.63.1.1
```
SadewaClient Network Configuration
```bash
auto eth0
iface eth0 inet static
	address 10.63.1.2
	netmask 255.255.255.0
	gateway 10.63.1.1
```
ArjunaLoadBalancer Network Configuration
```bash
auto eth0
iface eth0 inet static
	address 10.63.1.2
	netmask 255.255.255.0
	gateway 10.63.1.1
```

<h2>Switch2 Group</h2>

YudhistiraDNSMaster Network Configuration
```bash
auto eth0
iface eth0 inet static
	address 10.63.2.2
	netmask 255.255.255.0
	gateway 10.63.2.1
 ```
WerkudaraDNSMaster Network Configuration
```bash
auto eth0
iface eth0 inet static
	address 10.63.2.3
	netmask 255.255.255.0
	gateway 10.63.2.1
```

<h2>Switch3 Group</h2>

PrabukusumaWebServer
```bash
auto eth0
iface eth0 inet static
	address 10.63.3.2
	netmask 255.255.255.0
	gateway 10.63.3.1
```
AbimanyuWebServer
```bash
auto eth0
iface eth0 inet static
	address 10.63.3.3
	netmask 255.255.255.0
	gateway 10.63.3.1
```
WisanggeniWebServer
```bash
auto eth0
iface eth0 inet static
	address 10.63.3.4
	netmask 255.255.255.0
	gateway 10.63.3.1
```

<h1>Problem 2</h1>
<p>Buatlah website utama dengan akses ke arjuna.yyy.com dengan alias www.arjuna.yyy.com dengan yyy merupakan kode kelompok. So that makes our DNS arjuna.i09.com with alias www.arjuna.i09.com</p>

- First we need to do `apt-get update` and `apt install bind9 -y` in YudhistiraDNSMaster
- Then we do the command `nano /etc/bind/named.conf.local` then inside the nano, we do config as follows:

	```
	zone "arjuna.i09.com" {
		type master;
		file "/etc/bind/jarkom/arjuna.i09.com";
	};
	```
 - After that we do the steps below
   	- ```
   	  mkdir /etc/bind/jarkom
   	  ```
   	  ```
   	  cp /etc/bind/db.local /etc/bind/jarkom/arjuna.i09.com
   	  ```
   	  ```
   	  nano /etc/bind/jarkom/arjuna.i09.com
   	  ```
   	- and inside the nano file, we edit it as below:
   	  	<img src="https://media.discordapp.net/attachments/1153305482438660178/1161160822815473714/image.png?ex=65374a49&is=6524d549&hm=3158aeb5e704ab286818aa164be0955ed91da2b73932df54e2fa7fdde79bfa2d&=&width=690&height=445" alt="">
       - Restart the bind9 with the command `service bind9 restart`
 - Then we check if the DNS works in ArjunaLoadBalancer
   	- We first need to change the IP with `nano /etc/resolv.conf`
   	  	<img src="https://media.discordapp.net/attachments/1153305482438660178/1161163133650473020/image.png?ex=65374c70&is=6524d770&hm=02ecd4d998e02a844f9e6b915c5f2a8e1f81c2e3d6d0cbb70b15de9f147253a2&=&width=682&height=445" alt="">
   	- Then we ping it with `ping arjuna.i09.com -c 5`<br>
   	  	<img src="https://media.discordapp.net/attachments/1153305482438660178/1161163838792679454/image.png?ex=65374d18&is=6524d818&hm=078ee7f5ff7718295d5a9246827ace7685fc0eb27367cf267651aa4a5118d840&=&width=551&height=195" alt="">

- For the alias, we do the steps as before, but this time just add some commands into the nano file inside `/etc/bind/jarkom/arjuna.i09.com` then restart the bind9 as before<br>
  	<img src="https://media.discordapp.net/attachments/1153305482438660178/1161165207729610792/image.png?ex=65374e5e&is=6524d95e&hm=57c1c8048f16616993790b12b1ecc58150db398ad996f876a319e5b8465a5ec4&=&width=680&height=392" alt="">
- Then we check again for the DNS alias in ArjunaLoadBalancer
  	<img src="https://media.discordapp.net/attachments/1153305482438660178/1161165927396671498/image.png?ex=65374f0a&is=6524da0a&hm=5072a19c541e1f03b938865d331d62d0180414fc9a3fd0bba7817996ab443bc6&=&width=555&height=198" alt="">

<h1>Problem 3</h1>
<p>Dengan cara yang sama seperti soal nomor 2, buatlah website utama dengan akses ke abimanyu.yyy.com dan alias www.abimanyu.yyy.com.</p>

- The Procedure is the same as before so it is self-explanatory, the only difference is we add another command inside the `/etc/bind/named.conf.local` file.<br>
```
 zone "abimanyu.i09.com" {
 	type master;
 	file "/etc/bind/jarkom/abimanyu.i09.com";
 };
```
  <img src="https://media.discordapp.net/attachments/1153305482438660178/1161167381553479700/image.png?ex=65375065&is=6524db65&hm=d736f44a35c9e2771aa7f8dfccdc73a177e1636bacf250d1a83152c17f92d98b&=&width=679&height=397" alt="">

<h1>Problem 4</h1>
<p>Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.</p>

- We need to edit the file with `nano /etc/bind/jarkom/abimanyu.i09.com` in YudhistiraDNSMaster<br>
  <img src="https://media.discordapp.net/attachments/1153305482438660178/1161179765508685824/image.png?ex=65375bed&is=6524e6ed&hm=8ebde2278cd858baec50608c74ed464924ea9b3b1735f9e760dcab8567240284&=&width=684&height=395" alt="">
- Then we restart the bind9 with `service bind9 restart` command.<br>
- Now we can check if the subdomain works on the other client
  ```
  ping parikesit.abimanyu.i09.com -c 5
  ```
  <img src="https://media.discordapp.net/attachments/1153305482438660178/1161184057405743164/image.png?ex=65375fed&is=6524eaed&hm=c636375c484b6ea3a3dfdb80c41b841ad82e5ac8798cd176769faf01bd46a48c&=&width=580&height=196" alt="">

<h1>Problem 5</h1>
<p>Buat juga reverse domain untuk domain utama.</p>

- We are informed to make a main domain with `abimanyu.i09.com` as the one.
- Open the conf.local file using `nano /etc/bind/named.conf.local`
- add the command below inside the file.
  ```bash
  zone "2.63.10.in-addr.arpa" {
    type master;
    file "/etc/bind/jarkom/2.63.10.in-addr.arpa";
  };
  ```
   <img src="https://media.discordapp.net/attachments/1153305482438660178/1161452086823768094/image.png?ex=6538598c&is=6525e48c&hm=ba07be68b4529049df624c8830b86067a31f952f4f5d899773e1dda71515998a&=&width=681&height=454" alt="">
- We need to copy `db.local` and change the filename as follows;<br>
  ```
  cp /etc/bind/db.local /etc/bind/jarkom/2.63.10.in-addr.arpa
  ```
- We edit the file inside using `nano /etc/bind/jarkom/2.63.10.in-addr.arpa` command.<br>
  <img src="https://media.discordapp.net/attachments/1153305482438660178/1161203549426950176/image.png?ex=65377214&is=6524fd14&hm=4faeb0deb0e4f17297ad6f02cc7905d01936ade89c13e63163fd8c94528a6268&=&width=676&height=395" alt="">
- Then on another client, we do the steps below first.
	- direct the nameserver into the router again by using `echo nameserver 192.168.122.1 > /etc/resolv.conf`
  	- use `apt-get update`, then do `apt-get install dnsutils`
  	- after we do all that we go back to the YudhistiraDNSMaster again by using `echo nameserver 10.63.2.2 > /etc/resolv.conf`
- now we can check if the configuration is correct or not with the command<br>
  ```
  host -t PTR 10.63.2.2
  ```
  <img src="https://media.discordapp.net/attachments/1153305482438660178/1161205069149118484/image.png?ex=6537737e&is=6524fe7e&hm=1b66011416d1b25ae9ad95bd00e811eadb18fc1ddff9e8a52d0ad84daeaaad64&=&width=443&height=52" alt="">

<h1>Problem 6</h1>
<p>Agar dapat tetap dihubungi ketika DNS Server Yudhistira bermasalah, buat juga Werkudara sebagai DNS Slave untuk domain utama.</p>

- First thing to do is to configure the slave to be the WerkudaraDNSSlave by editing the file inside `/etc/bind/named.conf.local` in YudhistiraDNSMaster<br>
- Add the command below inside abimanyu.i09.com zone
  ```bash
  also-notify { 10.63.2.3; };
  allow-transfer { 10.63.2.3; };
  ```
- So the edited file will be like this,<br>
  <img src="https://media.discordapp.net/attachments/1153305482438660178/1161276083820310568/image.png?ex=6537b5a1&is=652540a1&hm=5783f174fbc7c8865c1303c6c28699d02fb8497d9d2aebc804c72396d734ae4b&=&width=474&height=200" alt="">
  ```
  zone "abimanyu.i09.com" {
	type master;
  	also-notify { 10.63.2.3; };
  	allow-transfer { 10.63.2.3; };
	file "/etc/bind/jarkom/abimanyu.i09.com";
  };
  ```
- Then we go to WerkudaraDNSSlave and do `apt-get update` also `apt-get install bind9 -y`<br>
- After that, we open the `/etc/bind/named.conf.local` on WerkudaraDNSSlave and add the command below<br>
  <img src="https://media.discordapp.net/attachments/1153305482438660178/1161260725331828847/image.png?ex=6537a754&is=65253254&hm=4f878b9dfcae6efcc87e81b42712c55c6eedfdacc93296178acc987681487869&=&width=675&height=389" alt="">
  ```
  zone "abimanyu.i09.com" {
    type slave;
    masters { 10.63.2.2; };
    file "/var/lib/bind/abimanyu.i09.com";
  };
  ```
- Don't forget to restart the bind9 with `service bind9 restart`
  <img src="https://media.discordapp.net/attachments/1153305482438660178/1161261096502571070/image.png?ex=6537a7ac&is=652532ac&hm=9ac62903bf2614afc8a1e6c4689ae18584ecc07b2220be2d9d2ddaf4974096b6&=&width=677&height=68" alt="">
- Now to test if the DNS slave works or not, we will stop the bind9 on YudhistiraDNSMaster with the command `service bind9 stop`
- In another Client we will check if it works, here we will change the nameserver to have both the master and the slave by configuring inside `/etc/resolv.conf` to be like below,<br>
  <img src="https://media.discordapp.net/attachments/1153305482438660178/1161266199775166514/image.png?ex=6537ac6d&is=6525376d&hm=4e1a21d224440ad9c6788623cc4476716856bb34b71ed209ffd25c4deb159bb2&=&width=677&height=405" alt="">
- If it works, the result will be like below,<br>
  <img src="https://media.discordapp.net/attachments/1153305482438660178/1161265953250742373/image.png?ex=6537ac32&is=65253732&hm=a61daf744d2726e7577b9d681733d3272fbac31f32e959798d4fed7470a6089d&=&width=546&height=206" alt="">
  
<h1>Problem 7</h1>
<p>Seperti yang kita tahu karena banyak sekali informasi yang harus diterima, buatlah subdomain khusus untuk perang yaitu baratayuda.abimanyu.yyy.com dengan alias www.baratayuda.abimanyu.yyy.com yang didelegasikan dari Yudhistira ke Werkudara dengan IP menuju ke Abimanyu dalam folder Baratayuda.</p>

- First we need to edit the `/etc/bind/jarkom/abimanyu.i09.com` on YudhistiraDNSMaster
  <img src="https://media.discordapp.net/attachments/1153305482438660178/1161482619419099237/image.png?ex=653875fb&is=652600fb&hm=6d278415f9adcb3404a3a264aa39bdf702e9dc51dbfed88aa9a226a78d71a897&=&width=679&height=456" alt="">
- Then we edit the `/etc/bind/named.conf.options` on YudhistiraDNSMaster to comment `dnssec-validation auto;` and also add the command as follows<br>

  ```
  allow-query{any;};
  ```
  <img src="https://media.discordapp.net/attachments/1153305482438660178/1161285134746660865/image.png?ex=6537be0f&is=6525490f&hm=1f9c2dd13d4f3f9f73532a8e4a9fb5b407484cd467ed63c7ed76e6cdfdbb95c0&=&width=680&height=405" alt="">
- Because we already have a DNSSLave with the command `allow-transfer { 10.63.2.3; };` there's no need to do any modification towards the file `/etc/bind/named.conf.local` in YudhistiraDNSMaster. But if it's necessary, then the command will be as below<br>
  <img src="https://media.discordapp.net/attachments/1153305482438660178/1161276083820310568/image.png?ex=6537b5a1&is=652540a1&hm=5783f174fbc7c8865c1303c6c28699d02fb8497d9d2aebc804c72396d734ae4b&=&width=474&height=200" alt="">
- Restart bind9 and configure the file in WerkudaraDNSSlave. using the command
  ```
  nano /etc/bind/named.conf.options
  ```
- Do the command as before by command `dnssec-validation auto;` and adding `allow-query{any;};`<br> in WerkudaraDNSSlave
- Now edit the inside of the file `/etc/bind/named.conf.local` with the command as follows;<br>
  ```
  nano /etc/bind/named.conf.local
  ```
  ```
  zone "baratayuda.abimanyu.i09.com" {
    type master;
    file "/etc/bind/delegasi/baratayuda.abimanyu.i09.com";
  };
  ```
  <img src="https://media.discordapp.net/attachments/1153305482438660178/1161458132241891328/image.png?ex=65385f2d&is=6525ea2d&hm=c016a92567238a0c01f2ad4747c93d5ad6600037a4dac0777fadd17f9e84c933&=&width=680&height=454" alt="">
- Make a directory and copy the `db.local` into the zone directory
  ```
  mkdir /etc/bind/delegasi
  cp /etc/bind/db.local /etc/bind/delegasi/baratayuda.abimanyu.i09.com
  ```
- Then edit the file as below<br>
  <img src="https://media.discordapp.net/attachments/1153305482438660178/1161513420756107374/image.png?ex=653892ab&is=65261dab&hm=6f57378ae745834171f43c19a72811e05e7cd9a0a12031083a072bb668cbbb17&=&width=677&height=452" alt="">
- Restart the bind9
  ```
  service bind9 restart
  ```
- run ping in another client
  ```
  ping baratayuda.abimanyu.i09.com -c 5
  ping www.baratayuda.abimanyu.i09.com -c 5
  ```
  <img src="https://media.discordapp.net/attachments/1153305482438660178/1161514714065551410/image.png?ex=653893df&is=65261edf&hm=20358bbd8e96684d622e4bb3c9040be7b1ab0fad1a8dca3c68ebfed139e78576&=&width=679&height=452" alt="">

<h1>Problem 8</h1>
<p>Untuk informasi yang lebih spesifik mengenai Ranjapan Baratayuda, buatlah subdomain melalui Werkudara dengan akses rjp.baratayuda.abimanyu.yyy.com dengan alias www.rjp.baratayuda.abimanyu.yyy.com yang mengarah ke Abimanyu.</p>

- all we do is go to the file in WerkudaraDNSSlave and add some commands<br>
  <img src="https://media.discordapp.net/attachments/1153305482438660178/1161541279545577492/image.png?ex=6538ac9d&is=6526379d&hm=6fcca6c96ae2de5a960ec2fa52c663b702a54313f570e118180d3e85630fdca0&=&width=680&height=451" alt="">
- Then we ping the DNS
  ```
  ping rjp.baratayuda.abimanyu.i09.com -c 5
  ping www.rjp.baratayuda.abimanyu.i09.com -c 5
  ```
  <img src="https://media.discordapp.net/attachments/1153305482438660178/1161542574620807189/image.png?ex=6538add2&is=652638d2&hm=473ecdb9d04cfdbfda9df68df0e8d670dd514a3279da48ec002583544a8dee2b&=&width=630&height=372">

<h1>Problem 9</h2>
<p>Arjuna merupakan suatu Load Balancer Nginx dengan tiga worker (yang juga menggunakan nginx sebagai webserver) yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Lakukan deployment pada masing-masing worker.</p>

- Now we will focus on the web server. the first thing to do is set up the workers, that is `PrabukusumaWebServer`,`AbimanyuWebServer`,`WisanggeniWebServer`
- In the `PrabukusumaWebServer` we will do as follows;
- Install Nginx and PHP and check the PHP version
 ```
 apt-get update && apt-get install nginx php php-fpm -y
 php -v
 ```
 <img src="https://media.discordapp.net/attachments/1153305482438660178/1161620159287541920/image.png?ex=6538f613&is=65268113&hm=61efa0c35ebe6b3ddb565a4cae28de9842eef23a02842d662137041d67c945fe&=&width=807&height=84">
- then make a new directory in `var/www` with the name `jarkom`

 ```
 mkdir /var/www/jarkom
 ```
- make a new file with the name `index.php` inside the directory and add the code below.<br>

 ```
 nano /var/www/jarkom/index.php
 ```
 ```php
  <?php
 echo "Hello, You're on Prabukusuma";
 ?>
 ```
- Then we are going to configure Nginx by doing
  ```
  nano /etc/nginx/sites-available/jarkom
  ```
- Now insert the server block configuration
  ```
  server {

	 	listen 80;
	
	 	root /var/www/jarkom;
	
	 	index index.php index.html index.htm;
	 	server_name _;
	
	 	location / {
	 			try_files $uri $uri/ /index.php?$query_string;
	 	}
	
	 	# pass PHP scripts to FastCGI server
	 	location ~ \.php$ {
	 	include snippets/fastcgi-php.conf;
	 	fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
	 	}
	
	 	location ~ /\.ht {
	 			deny all;
	 	}
	
	 	error_log /var/log/nginx/jarkom_error.log;
	 	access_log /var/log/nginx/jarkom_access.log;
 	}
  ```
- After we do all that, restart the Nginx and check if it runs well.
  ```
  service nginx restart
  ```
  ```
  nginx -t
  ```
