# Jarkom-Modul-5-F01-2022

Anggota:
1. Eldenabih Tavirazin Lutvie (5025201213)
2. Nouval Bachrezi (502520...)
3. Marsa A R (...)

## Soal (A)
Tugas pertama kalian yaitu membuat topologi jaringan sesuai dengan rancangan yang diberikan Loid dibawah ini: <br>
![image](https://user-images.githubusercontent.com/85897222/206858734-c464c4fe-b22f-4a9b-8a25-688cbaa8050e.png)
<br>
Keterangan : <br>	
Eden adalah DNS Server <br>
WISE adalah DHCP Server <br>
Garden dan SSS adalah Web Server <br>
Jumlah Host pada Forger adalah 62 host <br>
Jumlah Host pada Desmond adalah 700 host <br>
Jumlah Host pada Blackbell adalah 255 host <br>
Jumlah Host pada Briar adalah 200 host <br>
![image](https://user-images.githubusercontent.com/85897222/206860249-cd2beb45-27ba-4414-b2d3-12ac8e49335c.png)


## Soal (B)
Untuk menjaga perdamaian dunia, Loid ingin meminta kalian untuk membuat topologi tersebut menggunakan teknik CIDR atau VLSM setelah melakukan subnetting. <br>
### Pohon VLSM
![image](https://user-images.githubusercontent.com/85897222/206859104-512d5c46-7c5a-4300-8834-4d54167979af.png)
### Tabel Subnetting
![image](https://user-images.githubusercontent.com/85897222/206860205-85115feb-a3b9-4b46-a841-6bda3dd889a8.png)
<br>
![image](https://user-images.githubusercontent.com/85897222/206860188-afc5303e-dbef-4a9d-ba9b-59770ba6360e.png)

## Soal (C)
Anya, putri pertama Loid, juga berpesan kepada anda agar melakukan Routing agar setiap perangkat pada jaringan tersebut dapat terhubung. <br>
Konfigurasi tiap node:
### Strix
```
auto eth0
iface eth0 inet static
address 192.168.122.2
netmask 255.255.255.252
gateway 192.168.122.1

auto eth1
iface eth1 inet static
        address 10.29.0.1
        netmask 255.255.255.252

auto eth2
iface eth2 inet static
        address 10.29.0.5
        netmask 255.255.255.252
```

### Westalis
```
auto eth0
iface eth0 inet static
        address 10.29.0.2
        netmask 255.255.255.252
        gateway 10.29.0.1

auto eth1
iface eth1 inet static
        address 10.29.0.17
        netmask 255.255.255.248

auto eth2
iface eth2 inet static
        address 10.29.0.129
        netmask 255.255.255.128

auto eth3
iface eth3 inet static
        address 10.29.4.1
        netmask 255.255.252.0
```
### Ostania
```
auto eth0
iface eth0 inet static
        address 10.29.0.6
        netmask 255.255.255.252
        gateway 10.29.0.5

auto eth1
iface eth1 inet static
        address 10.29.1.1
        netmask 255.255.255.0

auto eth2
iface eth2 inet static
        address 10.29.0.25
        netmask 255.255.255.248

auto eth3
iface eth3 inet static
        address 10.29.2.1
        netmask 255.255.254.0
```
### Desmond, Forger, Blackbell, Briar
```
auto eth0
iface eth0 inet dhcp
```
### Eden
```
auto eth0
iface eth0 inet static
        address 10.29.0.18
        netmask 255.255.255.248
        gateway 10.29.0.17
```
### Wise
```
auto eth0
iface eth0 inet static
        address 10.29.0.19
        netmask 255.255.255.248
        gateway 10.29.0.17
```
### Garden
```
auto eth0
iface eth0 inet static
        address 10.29.0.27
        netmask 255.255.255.248
        gateway 10.29.0.25
```
### SSS
```
auto eth0
iface eth0 inet static
        address 10.29.0.26
        netmask 255.255.255.248
        gateway 10.29.0.25
```
Melakukan command-command di bawah ini untuk static routing dan default routing.
### Strix
```
route add -net 10.29.0.16 netmask 255.255.255.248 gw 10.29.0.2
route add -net 10.29.0.128 netmask 255.255.255.128 gw 10.29.0.2
route add -net 10.29.4.0 netmask 255.255.252.0 gw 10.29.0.2

route add -net 10.29.1.0 netmask 255.255.255.0 gw 10.29.0.6
route add -net 10.29.0.24 netmask 255.255.255.248 gw 10.29.0.6
route add -net 10.29.2.0 netmask 255.255.254.0 gw 10.29.0.6
```
### Ostania
```
route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.29.0.5
```
### Westalis
```
route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.29.0.1
```

## Soal (D)
Tugas berikutnya adalah memberikan ip pada subnet Forger, Desmond, Blackbell, dan Briar secara dinamis menggunakan bantuan DHCP server. Kemudian kalian ingat bahwa kalian harus setting DHCP Relay pada router yang menghubungkannya. <br>
Melakukan command-command untuk instalasi DHCP server, konfigurasi /etc/default/isc-dhcp-server, /etc/dhcp/dhcpd.conf, dan menjalankan DHCP Server seperti di bawah ini.
### Wise
```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf

apt-get update
apt-get install isc-dhcp-server -y

echo 'INTERFACES="eth0"' > '/etc/default/isc-dhcp-server'

echo '
# Forger A3
subnet 10.29.0.128 netmask 255.255.255.128 {
        range 10.29.0.130 10.29.0.254;
        option routers 10.29.0.129;
        option broadcast-address 10.29.0.255;
        option domain-name-servers 10.29.0.18;
        default-lease-time 600;
        max-lease-time 7200;
}

# Desmond A5
subnet 10.29.4.0 netmask 255.255.252.0 {
        range 10.29.4.2 10.29.7.254;
        option routers 10.29.4.1;
        option broadcast-address 10.29.7.255;
        option domain-name-servers 10.29.0.18;
        default-lease-time 600;
        max-lease-time 7200;
}

# Briar A7
subnet 10.29.1.0 netmask 255.255.255.0 {
        range 10.29.1.2 10.29.1.254;
        option routers 10.29.1.1;
        option broadcast-address 10.29.1.255;
        option domain-name-servers 10.29.0.18;
        default-lease-time 600;
        max-lease-time 7200;
}

# Blackbell A6
subnet 10.29.2.0 netmask 255.255.254.0 {
        range 10.29.2.2 10.29.3.254;
        option routers 10.29.2.1;
        option broadcast-address 10.29.3.255;
        option domain-name-servers 10.29.0.18;
        default-lease-time 600;
        max-lease-time 7200;
}

# WISE - Westalis A1
subnet 10.29.0.16 netmask 255.255.255.248 {
}
' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
service isc-dhcp-server restart
```
Melakukan command-command di bawah ini untuk dhcp relay seperti instalasi DHCP Relay dan konfigurasi file /etc/default/isc-dhcp-relay.
### Westalis dan Ostania
```
echo '
nameserver 192.168.122.1
' > /etc/resolv.conf

apt-get update
apt-get install isc-dhcp-relay -y

echo '
SERVERS="10.29.0.19"
INTERFACES="eth0 eth1 eth2 eth3"
OPTIONS=
' > /etc/default/isc-dhcp-relay

service isc-dhcp-relay restart
service isc-dhcp-relay restart
```
Melakukan command-command di bawah ini untuk DNS forwarder.
### Eden
```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf

apt-get update
apt-get install bind9 -y

echo '
options {
        directory "/var/cache/bind";
        forwarders {
                192.168.122.1;
        };
        // dnssec-validation auto;
        allow-query{any;};
        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};
' > /etc/bind/named.conf.options

service bind9 restart
service bind9 restart
```
### Testing


## Soal 1
Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Strix menggunakan iptables, tetapi Loid tidak ingin menggunakan MASQUERADE. <br>

## Soal 2
Kalian diminta untuk melakukan drop semua TCP dan UDP dari luar Topologi kalian pada server yang merupakan DHCP Server demi menjaga keamanan. <br>
## Soal 3
Loid meminta kalian untuk membatasi DHCP dan DNS Server hanya boleh menerima maksimal 2 koneksi ICMP secara bersamaan menggunakan iptables, selebihnya didrop. <br>
## Soal 4
Akses menuju Web Server hanya diperbolehkan disaat jam kerja yaitu Senin sampai Jumat pada pukul 07.00 - 16.00. <br>
## Soal 5
Karena kita memiliki 2 Web Server, Loid ingin Ostania diatur sehingga setiap request dari client yang mengakses Garden dengan port 80 akan didistribusikan secara bergantian pada SSS dan Garden secara berurutan dan request dari client yang mengakses SSS dengan port 443 akan didistribusikan secara bergantian pada Garden dan SSS secara berurutan. <br>
## Soal 6
Karena Loid ingin tau paket apa saja yang di-drop, maka di setiap node server dan router ditambahkan logging paket yang di-drop dengan standard syslog level. <br>

### Testing

## Kendala
tidak ada
