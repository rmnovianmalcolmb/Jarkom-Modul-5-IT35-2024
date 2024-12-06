# Jarkom-Modul-5-IT35-2024

| Nama          | NRP          |
| ------------- | ------------ |
| RM Novian Malcolm Bayuputra | 5027231035 |
| Nayyara Ashila | 5027231083 |


## Topologi Jaringan dan Pembagian Subnet

![Group 1 (1)](https://github.com/user-attachments/assets/1ac9d547-72d5-4473-9237-3d6f3da51eb9)

## Tree

![treemodul5 drawio (1)](https://github.com/user-attachments/assets/a2d92299-b5db-479c-aa67-df9bbed6fc16)

## Pembagian IP

[Pembagian IP](https://docs.google.com/spreadsheets/d/13C_j3KuroBReQDuPBjBSuERxCKTzHEmH_HTmGbrXbgM/edit?gid=671499381#gid=671499381)

## Network Configuration

### NewEridu
```
#NAT
auto eth0
iface eth0 inet dhcp

#A1
auto eth1
iface eth1 inet static
	address 192.234.2.217
	netmask 255.255.255.252

#A5
auto eth2
iface eth2 inet static
	address 192.234.2.221
	netmask 255.255.255.252
```

### LuminaSquare
```
#A1
auto eth0
iface eth0 inet static
	address 192.234.2.218
	netmask 255.255.255.252
    gateway 192.234.2.217

#A2
auto eth1
iface eth1 inet static
	address 192.234.2.193
	netmask 255.255.255.248

#A4
auto eth2
iface eth2 inet static
	address 192.234.1.1
	netmask 255.255.255.0
```

### BalletTwins
```
#A2
auto eth0
iface eth0 inet static
	address 192.234.2.195
	netmask 255.255.255.248
    gateway 192.234.2.193

#A3
auto eth1
iface eth1 inet static
	address 192.234.2.1
	netmask 255.255.255.128
```

### SixStreet
```
#A5
auto eth0
iface eth0 inet static
	address 192.234.2.222
	netmask 255.255.255.252
    gateway 192.234.2.221

#A6
auto eth2
iface eth2 inet static
	address 192.234.2.201
	netmask 255.255.255.248

#A9
auto eth1
iface eth1 inet static
	address 192.234.2.209
	netmask 255.255.255.248
```

### ScootOutpost
```
#A6
auto eth0
iface eth0 inet static
	address 192.234.2.203
	netmask 255.255.255.248
    gateway 192.234.2.201

#A7
auto eth1
iface eth1 inet static
	address 192.234.2.225
	netmask 255.255.255.252
```

### OuterRing
```
#A6
auto eth0
iface eth0 inet static
	address 192.234.2.202
	netmask 255.255.255.248
    gateway 192.234.2.201

#A8
auto eth1
iface eth1 inet static
	address 192.234.2.129
	netmask 255.255.255.192
```

### HIA
```
auto eth0
iface eth0 inet static
	address 192.234.2.194
	netmask 255.255.255.248
    gateway 192.234.2.193
```

### HDD
```
auto eth0
iface eth0 inet static
	address 192.234.2.210
	netmask 255.255.255.248
    gateway 192.234.2.209
```

### Fairy
```
auto eth0
iface eth0 inet static
	address 192.234.2.211
	netmask 255.255.255.248
    gateway 192.234.2.209
```

### HollowZero
```
auto eth0
iface eth0 inet static
	address 192.234.2.226
	netmask 255.255.255.252
    gateway 192.234.2.225
```

### Client (Caesar, Burnice, Jane, Policeboo, Ellen, Lycaon)
```
auto eth0
iface eth0 inet dhcp
```

## .bashrc
### NewEridu
```
#A2
route add -net 192.234.2.192 netmask 255.255.255.248 gw 192.234.2.218

#A3
route add -net 192.234.2.0 netmask 255.255.255.128 gw 192.234.2.218

#A4
route add -net 192.234.1.0 netmask 255.255.255.0 gw 192.234.2.218

#A6
route add -net 192.234.2.200 netmask 255.255.255.248 gw 192.234.2.222

#A7
route add -net 192.234.2.224 netmask 255.255.255.252 gw 192.234.2.222

#A8
route add -net 192.234.2.128 netmask 255.255.255.192 gw 192.234.2.222

#A9
route add -net 192.234.2.208 netmask 255.255.255.248 gw 192.234.2.222

IP_ETH0=$(ip -4 addr show eth0 | grep -oP '(?<=inet\s)\d+(\.\d+){3}')
iptables -t nat -A POSTROUTING -o eth0 -j SNAT --to-source $IP_ETH0
```

### LuminaSquare (DHCP Relay)
```
#A3
route add -net 192.234.2.0 netmask 255.255.255.128 gw 192.234.2.195

#A7
route add -net 192.234.2.224 netmask 255.255.255.252 gw 192.234.2.217

#A8
route add -net 192.234.2.128 netmask 255.255.255.192 gw 192.234.2.217

#A9
route add -net 192.234.2.208 netmask 255.255.255.248 gw 192.234.2.217

echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update
apt install isc-dhcp-relay -y

echo 'SERVERS="192.234.2.211"
INTERFACES="eth0 eth1 eth2 eth3"
OPTIONS=""
' > /etc/default/isc-dhcp-relay

echo 'net.ipv4.ip_forward=1' >> /etc/sysctl.conf

service isc-dhcp-relay restart
```

### BalletTwins (DHCP Relay)
```
#A4
route add -net 192.234.1.0 netmask 255.255.255.0 gw 192.234.2.193

#A7
route add -net 192.234.2.224 netmask 255.255.255.252 gw 192.234.2.193

#A8
route add -net 192.234.2.128 netmask 255.255.255.192 gw 192.234.2.193

#A9
route add -net 192.234.2.208 netmask 255.255.255.248 gw 192.234.2.193

echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update
apt install isc-dhcp-relay -y

echo 'SERVERS="192.234.2.211"
INTERFACES="eth0 eth1 eth2 eth3"
OPTIONS=""
' > /etc/default/isc-dhcp-relay

echo 'net.ipv4.ip_forward=1' >> /etc/sysctl.conf

service isc-dhcp-relay restart
```

### SixStreet (DHCP Relay)
```
#A2
route add -net 192.234.2.192 netmask 255.255.255.248 gw 192.234.2.221

#A3
route add -net 192.234.2.0 netmask 255.255.255.128 gw 192.234.2.221

#A4
route add -net 192.234.1.0 netmask 255.255.255.0 gw 192.234.2.221

#A7
route add -net 192.234.2.224 netmask 255.255.255.252 gw 192.234.2.203

#A8
route add -net 192.234.2.128 netmask 255.255.255.192 gw 192.234.2.202

echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update
apt install isc-dhcp-relay -y

echo 'SERVERS="192.234.2.211"
INTERFACES="eth0 eth1 eth2 eth3"
OPTIONS=""
' > /etc/default/isc-dhcp-relay

echo 'net.ipv4.ip_forward=1' >> /etc/sysctl.conf

service isc-dhcp-relay restart
```

### OuterRing (DHCP Relay)
```
#A2
route add -net 192.234.2.192 netmask 255.255.255.248 gw 192.234.2.201

#A3
route add -net 192.234.2.0 netmask 255.255.255.128 gw 192.234.2.201

#A4
route add -net 192.234.1.0 netmask 255.255.255.0 gw 192.234.2.201

#A7
route add -net 192.234.2.224 netmask 255.255.255.252 gw 192.234.2.203

#A9
route add -net 192.234.2.208 netmask 255.255.255.248 gw 192.234.2.201

echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update
apt install isc-dhcp-relay -y

echo 'SERVERS="192.234.2.211"
INTERFACES="eth0 eth1 eth2 eth3"
OPTIONS=""
' > /etc/default/isc-dhcp-relay

echo 'net.ipv4.ip_forward=1' >> /etc/sysctl.conf

service isc-dhcp-relay restart
```

### ScootOutpost
```
#A2
route add -net 192.234.2.192 netmask 255.255.255.248 gw 192.234.2.201

#A3
route add -net 192.234.2.0 netmask 255.255.255.128 gw 192.234.2.201

#A4
route add -net 192.234.1.0 netmask 255.255.255.0 gw 192.234.2.201

#A8
route add -net 192.234.2.128 netmask 255.255.255.192 gw 192.234.2.202

#A9
route add -net 192.234.2.208 netmask 255.255.255.248 gw 192.234.2.201

echo 'nameserver 192.168.122.1' > /etc/resolv.conf

Fairy (DHCP Server)
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update
apt-get install isc-dhcp-server netcat -y

echo 'INTERFACESv4="eth0"' > /etc/default/isc-dhcp-server

echo '#A8
subnet 192.234.2.128 netmask 255.255.255.192 {
        range 192.234.2.130 192.234.2.190;
        option routers 192.234.2.129; # Gateway
        option broadcast-address 192.234.2.191;
        option domain-name-servers 192.234.2.210; #IP HDD
        default-lease-time 600;
        max-lease-time 7200;
}

#A4
subnet 192.234.1.0 netmask 255.255.255.0 {
        range 192.234.1.2 192.234.1.254;
        option routers 192.234.1.1;
        option broadcast-address 192.234.1.255;
        option domain-name-servers 192.234.2.210;
        default-lease-time 600;
        max-lease-time 7200;
}

#A3
subnet 192.234.2.0 netmask 255.255.255.128 {
        range 192.234.2.2 192.234.2.126;
        option routers 192.234.2.1;
        option broadcast-address 192.234.2.127;
        option domain-name-servers 192.234.2.210;
        default-lease-time 600;
        max-lease-time 7200;
}

subnet 192.234.2.216 netmask 255.255.255.252 {}
subnet 192.234.2.192 netmask 255.255.255.248 {}
subnet 192.234.2.220 netmask 255.255.255.252 {}
subnet 192.234.2.200 netmask 255.255.255.248 {}
subnet 192.234.2.224 netmask 255.255.255.252 {}
subnet 192.234.2.208 netmask 255.255.255.248 {}
' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
```

### HDD (DNS Server)
```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update
apt-get install bind9 netcat -y

echo 'options {
        directory "/var/cache/bind";

        forwarders {
                192.168.122.1;
        };

        // dnssec-validation auto;
        allow-query{any;};
        auth-nxdomain no;
        listen-on-v6 { any; };
}; ' >/etc/bind/named.conf.options

service bind9 restart
```

### HIA & HollowZero (Apache Worker)
```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update
apt-get install apache2 netcat -y

service apache2 start

echo 'Welcome to {hostname}' > /var/www/html/index.html

service apache2 restart
```

## Misi 2

### 1.
![image](https://github.com/user-attachments/assets/ea0e169a-c91d-40e9-8832-2bb015c266bc)
```
iptables -t nat -A POSTROUTING -o eth0 -j SNAT --to-source $IP_ETH0
```

### 2.

![image](https://github.com/user-attachments/assets/a6f30119-025f-4123-836c-b317d1956dc4)

Fairy
```
# Blokir semua ping ke Fairy
iptables -A INPUT -p icmp --icmp-type echo-request -j DROP

#Perbolehkan ping dari Fairy
iptables -A OUTPUT -p icmp --icmp-type echo-request -j ACCEPT
```

![image](https://github.com/user-attachments/assets/38261090-dd37-44be-a49e-dcbb778692fe)

![image](https://github.com/user-attachments/assets/4d53f2b0-2613-4148-bd9d-992959d9ef85)

### 3.

![image](https://github.com/user-attachments/assets/4ac41adc-e1ba-4901-bbce-d34bcf6e8899)

HDD
```
#Izinkan koneksi dari Fairy
iptables -A INPUT -s 192.246.2.211 -j ACCEPT

#Tolak semua koneksi dari sumber lain
iptables -A INPUT -j REJECT
```

![image](https://github.com/user-attachments/assets/06c167ec-e5f5-455f-9182-955ce9f9075e)

![image](https://github.com/user-attachments/assets/db4dab41-ec3c-43ab-afe3-b831eff8232f)

![image](https://github.com/user-attachments/assets/cbc5f141-dd6e-48ca-ae20-eb4324ac4184)

### 4.

![image](https://github.com/user-attachments/assets/4c9f00ce-865f-4f18-9e02-a013b6e1a731)

HollowZero
```
# Burnice Caesar Jane Policeboo
iptables -A INPUT -p tcp -s (IP Burnice) --dport 80 -m time --timestart 00:00 --timestop 23:59 --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT

iptables -A INPUT -p tcp -s (IP Caesar) --dport 80 -m time --timestart 00:00 --timestop 23:59 --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT

iptables -A INPUT -p tcp -s (IP Jane) --dport 80 -m time --timestart 00:00 --timestop 23:59 --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT

iptables -A INPUT -p tcp -s (IP Policeboo) --dport 80 -m time --timestart 00:00 --timestop 23:59 --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT

iptables -A INPUT -p tcp --dport 80 -j REJECT
```

![image](https://github.com/user-attachments/assets/08b82013-1575-4836-b0d2-7f5b50f3ac00)

![image](https://github.com/user-attachments/assets/58781051-a567-4c8e-a93a-3ccd422489c9)

### 5.

![image](https://github.com/user-attachments/assets/094d7e62-5553-41d8-8cae-06519900449d)

HIA
```
# Akses Node Ellen dan Lycaon (08:00 - 21:00)
iptables -A INPUT -p tcp -s (IP Ellen) --dport 80 -m time --timestart 01:00 --timestop 14:00 --weekdays Mon,Tue,Wed,Thu,Fri,Sat,Sun -j ACCEPT

iptables -A INPUT -p tcp -s (IP Lycaon) --dport 80 -m time --timestart 01:00 --timestop 14:00 --weekdays Mon,Tue,Wed,Thu,Fri,Sat,Sun -j ACCEPT

# Akses Node Jane dan Policeboo (03:00 - 23:00)
iptables -A INPUT -p tcp -s (IP Jane) --dport 80 -m time --timestart 20:00 --timestop 16:00 --weekdays Mon,Tue,Wed,Thu,Fri,Sat,Sun -j ACCEPT

iptables -A INPUT -p tcp -s (IP Policeboo) --dport 80 -m time --timestart 20:00 --timestop 16:00 --weekdays Mon,Tue,Wed,Thu,Fri,Sat,Sun -j ACCEPT

# Tolak semua koneksi lainnya
iptables -A INPUT -p tcp --dport 80 -j REJECT
```

![image](https://github.com/user-attachments/assets/a2c8dc5e-aebc-4583-892f-29990b007a84)

![image](https://github.com/user-attachments/assets/435ee563-c14e-4119-a742-1fdf0bddf781)

### 6.

![image](https://github.com/user-attachments/assets/e7d877bf-a17f-4754-8d93-8156a8aa9195)

HIA
```
# Atur rate limit untuk port scanning (maksimum 25 koneksi per 10 detik)
iptables -N PORTSCAN
iptables -A INPUT -p tcp --dport 1:100 -m state --state NEW -m recent --set --name portscan
iptables -A INPUT -p tcp --dport 1:100 -m state --state NEW -m recent --update --seconds 10 --hitcount 25 --name portscan -j PORTSCAN

# Blokir IP yang terdeteksi melakukan port scanning tidak wajar
iptables -A PORTSCAN -m recent --set --name blacklist
iptables -A PORTSCAN -j DROP

# Blokir semua aktivitas dari IP yang ada di daftar blacklist
iptables -A INPUT -m recent --name blacklist --rcheck -j REJECT
iptables -A OUTPUT -m recent --name blacklist --rcheck -j REJECT

# Logging untuk port scanning
iptables -A PORTSCAN -j LOG --log-prefix='PORT SCAN DETECTED' --log-level 4
```

[Hasil analisis](https://docs.google.com/document/d/1sqsGdkdYZDqCCbIJ1N6nwNeuJ2b5brRPjhPlgvuVF3c/edit?usp=sharing)

### 7.

![image](https://github.com/user-attachments/assets/9df542c9-605b-4a86-917e-7f2c276e0295)

HollowZero
```
# Hanya izinkan maksimal 2 koneksi aktif dari 2 IP berbeda
iptables -A INPUT -p tcp --dport 80 -m conntrack --ctstate NEW -m recent --set
iptables -A INPUT -p tcp --dport 80 -m conntrack --ctstate NEW -m recent --update --seconds 1 --hitcount 3 -j REJECT
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```

![image](https://github.com/user-attachments/assets/1f148931-21dd-406e-a95c-0449bc827515)

### 8.

![image](https://github.com/user-attachments/assets/f836dacb-0926-44df-a337-ba9c7dc7a0b4)

Burnice
```
iptables -t nat -A PREROUTING -p tcp -j DNAT --to-destination 192.234.2.226 --dport 8080
iptables -A FORWARD -p tcp -d 192.234.2.226 -j ACCEPT
```

![image](https://github.com/user-attachments/assets/e0d7d65f-ad3b-4324-a3e4-5360a7e308c3)

![image](https://github.com/user-attachments/assets/42d21271-1fb8-4977-8ce4-83c7b54cd296)

![Screenshot 2024-12-06 223246](https://github.com/user-attachments/assets/f2651d77-769e-449f-861a-6484dd851f50)

### 9.

![image](https://github.com/user-attachments/assets/9ccf2edd-cb36-4459-88b3-03489808a436)

Burnice
```
iptables --policy INPUT DROP
iptables --policy OUTPUT DROP
iptables --policy FORWARD DROP
```

![image](https://github.com/user-attachments/assets/e9b366b0-ac4c-468c-a9a5-472c290fdf4d)

![image](https://github.com/user-attachments/assets/6c0090c0-1c60-486d-850a-c2bc9fb0fedd)

