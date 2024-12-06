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


