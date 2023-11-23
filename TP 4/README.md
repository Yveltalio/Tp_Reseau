# Quatrième TP 🐱

## I. DHCP Client

## 🌞 Adresses a déterminer

### Adresseur serveur DHCP : 10.33.79.254

### Bail Obtenu : vendredi 27 octobre 2023 8h57

### Bail Expirant : samedi 28 octobre 2023 8h57

## 🌞 Capturer un échange DHCP

[Voir fichier ici](./tp4_dhcp_client.pcapng)

## II. Serveur DHCP

### On ping avec le DHCP google.com

```PowerShell
[aymeric@localhost ~]$ ping google.com
PING google.com (172.217.18.206) 56(84) bytes of data.
64 bytes from par10s38-in-f14.1e100.net (172.217.18.206): icmp_seq=1 ttl=115 time=19.0 ms
64 bytes from par10s38-in-f14.1e100.net (172.217.18.206): icmp_seq=2 ttl=115 time=18.3 ms
64 bytes from par10s38-in-f14.1e100.net (172.217.18.206): icmp_seq=3 ttl=115 time=18.1 ms
64 bytes from par10s38-in-f14.1e100.net (172.217.18.206): icmp_seq=4 ttl=115 time=16.9 ms
^C64 bytes from 172.217.18.206: icmp_seq=5 ttl=115 time=19.2 ms
```

### On ping avec node2 google.com

```PowerShell
[aymeric@localhost ~]$ ping google.com 
PING google.com (172.217.18.206) 56(84) bytes of data.
64 bytes from ham02s14-in-f206.1e100.net (172.217.18.206): icmp_seq=1 ttl=115 time=18.5 ms
64 bytes from par10s38-in-f14.1e100.net (172.217.18.206): icmp_seq=2 ttl=115 time=17.8 ms
64 bytes from par10s38-in-f14.1e100.net (172.217.18.206): icmp_seq=3 ttl=115 time=18.7 ms
64 bytes from par10s38-in-f14.1e100.net (172.217.18.206): icmp_seq=4 ttl=115 time=19.6 ms
^C64 bytes from 172.217.18.206: icmp_seq=5 ttl=115 time=18.7 ms
```

### Trace route google.com depuis node2

```PowerShell
[aymeric@localhost ~]$ traceroute google.com
traceroute to google.com (172.217.18.206), 30 hops max, 60 byte packets
 1  _gateway (10.4.1.254)  11.564 ms  10.383 ms  9.973 ms
 2  10.0.3.2 (10.0.3.2)  8.918 ms  8.322 ms  8.035 ms
 3  10.0.3.2 (10.0.3.2)  7.320 ms  6.707 ms  4.674 ms
```

## 4.Serveur DHCP

### 🌞 Rendu


#### 1  sudo dnf -y install dhcp-server  
#### 2  sudo nano /etc/dhcp/dhcpd.conf
#### 3  sudo systemctl enable --now dhcpd
#### 4  sudo firewall-cmd --add-service=dhcp
#### 5  sudo firewall-cmd --runtime-to-permanent

```PowerShell
[aymeric@localhost ~]$ systemctl status dhcpd
● dhcpd.service - DHCPv4 Server Daemon
     Loaded: loaded (/usr/lib/systemd/system/dhcpd.service; enabled; preset: disabled)
     Active: active (running) since Fri 2023-10-27 15:48:02 CEST; 5min ago
       Docs: man:dhcpd(8)
             man:dhcpd.conf(5)
   Main PID: 1888 (dhcpd)
     Status: "Dispatching packets..."
      Tasks: 1 (limit: 4674)
     Memory: 4.6M
        CPU: 10ms   
     CGroup: /system.slice/dhcpd.service
             └─1888 /usr/sbin/dhcpd -f -cf /etc/dhcp/dhcpd.conf -user dhcpd -group dhcpd --no-pid

Oct 27 15:48:02 localhost.localdomain dhcpd[1888]: Sending on   LPF/enp0s3/08:00:27:4f:1d:2f/10.4.1.0/24
Oct 27 15:48:02 localhost.localdomain dhcpd[1888]: Sending on   Socket/fallback/fallback-net
Oct 27 15:48:02 localhost.localdomain dhcpd[1888]: Server starting service.
Oct 27 15:48:02 localhost.localdomain systemd[1]: Started DHCPv4 Server Daemon.
Oct 27 15:49:48 localhost.localdomain dhcpd[1888]: DHCPDISCOVER from 08:00:27:c8:fa:82 via enp0s3
Oct 27 15:49:55 localhost.localdomain dhcpd[1888]: DHCPOFFER on 10.4.1.137 to 08:00:27:c8:fa:82 via enp0s3
Oct 27 15:49:55 localhost.localdomain dhcpd[1888]: DHCPDISCOVER from 08:00:27:c8:fa:82 via enp0s3
Oct 27 15:49:55 localhost.localdomain dhcpd[1888]: DHCPOFFER on 10.4.1.137 to 08:00:27:c8:fa:82 via enp0s3
Oct 27 15:49:55 localhost.localdomain dhcpd[1888]: DHCPREQUEST for 10.4.1.137 (10.4.1.253) from 08:00:27:c8:fa:82 via enp0s3
Oct 27 15:49:55 localhost.localdomain dhcpd[1888]: DHCPACK on 10.4.1.137 to 08:00:27:c8:fa:82 via enp0s3
```

```PowerShell
[aymeric@localhost ~]$ sudo cat /etc/dhcp/dhcpd.conf
option domain-name     "srv.world";
option domain-name-servers     dlp.srv.world;
default-lease-time 600;
max-lease-time 7200;
authoritative;
subnet 10.4.1.0 netmask 255.255.255.0 {
    range dynamic-bootp 10.4.1.137 10.4.1.147;
    option broadcast-address 10.4.1.255;
    option routers 10.4.1.254;
}
```

## 5.Client DHCP

### 🌞 Après le changement d'ip automatique par le DHCP

```PowerShell
[aymeric@localhost ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:c8:fa:82 brd ff:ff:ff:ff:ff:ff
    inet 10.4.1.137/24 brd 10.4.1.255 scope global dynamic noprefixroute enp0s3
       valid_lft 408sec preferred_lft 408sec
    inet6 fe80::a00:27ff:fec8:fa82/64 scope link
       valid_lft forever preferred_lft forever
```

### Baux du clien

```PowerShell
[aymeric@localhost ~]$ nmcli con show enp0s3
connection.timestamp:                   1698415888
```
> Debut du bail : 07/11/2023 17:53:04

```PowerShell
DHCP4.OPTION[7]:                        expiry = 1699375984

```

> Fin du bail : 07/11/2023 22:44:4

```PowerShell
DHCP4.OPTION[4]:                        dhcp_server_identifier = 10.4.1.253

```
### 🌞 Ping de node 1 a node 2

```PowerShell
[aymeric@localhost ~]$ ping 10.4.1.12
PING 10.4.1.12 (10.4.1.12) 56(84) bytes of data.
64 bytes from 10.4.1.12: icmp_seq=1 ttl=64 time=1.91 ms
64 bytes from 10.4.1.12: icmp_seq=2 ttl=64 time=1.36 ms
64 bytes from 10.4.1.12: icmp_seq=3 ttl=64 time=1.08 ms
64 bytes from 10.4.1.12: icmp_seq=4 ttl=64 time=1.33 ms
64 bytes from 10.4.1.12: icmp_seq=5 ttl=64 time=1.52 ms

```

### 🌞Fichier bail dhcp

```PowerShell

[aymeric@localhost ~]$ cat /var/lib/dhcpd/dhcpd.leases
# The format of this file is documented in the dhcpd.leases(5) manual page.
# This lease file was written by isc-dhcp-4.4.2b1

# authoring-byte-order entry is generated, DO NOT DELETE
authoring-byte-order little-endian;

lease 10.4.1.137 {
  starts 0 2023/10/29 10:55:57;
  ends 0 2023/10/29 16:55:57;
  tstp 0 2023/10/29 16:55:57;
  cltt 0 2023/10/29 10:55:57;
  binding state free;
  hardware ethernet 08:00:27:c8:fa:82;
  uid "\001\010\000'\310\372\202";
}
server-duid "\000\001\000\001,\320\365\355\010\000'O\035/";

lease 10.4.1.137 {
  starts 2 2023/11/07 16:42:44;
  ends 2 2023/11/07 22:42:44;
  cltt 2 2023/11/07 16:42:44;
  binding state active;
  next binding state free;
  rewind binding state free;
  hardware ethernet 08:00:27:c8:fa:82;
  uid "\001\010\000'\310\372\202";
}
```

### Après changements des parametres du serveur 

```PowerShell
[aymeric@localhost ~]$ sudo cat /etc/dhcp/dhcpd.conf
option domain-name     "srv.world";
option domain-name-servers     8.8.8.8;
default-lease-time 21600;
max-lease-time 22000;
authoritative;
subnet 10.4.1.0 netmask 255.255.255.0 {
    range dynamic-bootp 10.4.1.137 10.4.1.147;
    option broadcast-address 10.4.1.255;
    option routers 10.4.1.254;
}

```
### Redemander une adresse ip
> sudo dhclient -r enp0s3  
>sudo dhclient enp0s3

### Adresse DNS enregistré :

```PowerShell
[aymeric@localhost /]$ cat etc/resolv.conf
; generated by /usr/sbin/dhclient-script
search srv.world
nameserver 8.8.8.8

```
### Route par défaut :

```PowerShell
[aymeric@localhost /]$ ip r s
default via 10.4.1.254 dev enp0s3
default via 10.4.1.254 dev enp0s3 proto dhcp src 10.4.1.137 metric 100
10.4.1.0/24 dev enp0s3 proto kernel scope link src 10.4.1.137 metric 100

```
### Durée de bail de 6h :
```

lease 10.4.1.138 {
  starts 2 2023/11/07 17:29:39;
  ends 2 2023/11/07 23:29:39;
  cltt 2 2023/11/07 17:29:39;
  binding state active;
  next binding state free;
  rewind binding state free;
  hardware ethernet 08:00:27:c8:fa:82;
}

```

### 🦈Capture WireShark

[Voir fichier ici](./trame_dhcp_client_fin.pcapng)