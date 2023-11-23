# Cinquième TP 🐱

## 2. Setup

### 🌞Fichier conf principal

```PowerShell
[aymeric@dns ~]$  sudo cat /etc/named.conf
options {
        listen-on port 53 { 127.0.0.1; any; };
        listen-on-v6 port 53 { ::1; };
        directory       "/var/named";
        dump-file       "/var/named/data/cache_dump.db";
        statistics-file "/var/named/data/named_stats.txt";
        memstatistics-file "/var/named/data/named_mem_stats.txt";
        secroots-file   "/var/named/data/named.secroots";
        recursing-file  "/var/named/data/named.recursing";
        allow-query     { localhost; any; };
        allow-query-cache { localhost; any; };

        recursion yes;

        dnssec-validation yes;

        managed-keys-directory "/var/named/dynamic";
        geoip-directory "/usr/share/GeoIP";

        pid-file "/run/named/named.pid";
        session-keyfile "/run/named/session.key";

        /* https://fedoraproject.org/wiki/Changes/CryptoPolicy */
        include "/etc/crypto-policies/back-ends/bind.config";
};

logging {
        channel default_debug {
                file "data/named.run";
                severity dynamic;
        };
};

zone "." IN {
        type hint;
        file "named.ca";
};

include "/etc/named.rfc1912.zones";
include "/etc/named.root.key";

zone "tp6.b1" IN {
     type master;
     file "tp6.b1.db";
     allow-update { none; };
     allow-query {any; };
};
zone "1.4.10.in-addr.arpa" IN {
     type master;
     file "tp6.b1.rev";
     allow-update { none; };
     allow-query { any; };
};
```

### 🌞Premier fichier de zone

```PowerShell
$TTL 86400
@ IN SOA dns.tp6.b1. admin.tp6.b1. (
    2019061800 ;Serial
    3600 ;Refresh
    1800 ;Retry
    604800 ;Expire
    86400 ;Minimum TTL
)

; Infos sur le serveur DNS lui même (NS = NameServer)
@ IN NS dns.tp6.b1.

; Enregistrements DNS pour faire correspondre des noms à des IPs
dns       IN A 10.6.1.101
john      IN A 10.6.1.11
```


### 🌞Deuxième fichier de zone 


```PowerShell
[aymeric@dns ~]$ sudo cat /var/named/tp6.b1.rev
$TTL 86400
@ IN SOA dns.tp6.b1. admin.tp6.b1. (
    2019061800 ;Serial
    3600 ;Refresh
    1800 ;Retry
    604800 ;Expire
    86400 ;Minimum TTL
)

; Infos sur le serveur DNS lui même (NS = NameServer)
@ IN NS dns.tp6.b1.

; Reverse lookup
101 IN PTR dns.tp6.b1.
11 IN PTR john.tp6.b1.
```

### Status du DNS

```PowerShell
[aymeric@dns ~]$ systemctl status named
● named.service - Berkeley Internet Name Domain (DNS)
     Loaded: loaded (/usr/lib/systemd/system/named.service; enabled; preset: disabled)
     Active: active (running) since Fri 2023-11-17 16:12:28 CET; 5min ago
   Main PID: 1973 (named)
      Tasks: 5 (limit: 4674)
     Memory: 20.2M
        CPU: 57ms
     CGroup: /system.slice/named.service
             └─1973 /usr/sbin/named -u named -c /etc/named.conf

Nov 17 16:12:28 dns.tp6.b1 named[1973]: network unreachable resolving './NS/IN': 2001:7fe::53#53
Nov 17 16:12:28 dns.tp6.b1 named[1973]: zone 1.0.0.127.in-addr.arpa/IN: loaded serial 0
Nov 17 16:12:28 dns.tp6.b1 named[1973]: zone tp6.b1/IN: loaded serial 2019061800
Nov 17 16:12:28 dns.tp6.b1 named[1973]: zone localhost/IN: loaded serial 0
Nov 17 16:12:28 dns.tp6.b1 named[1973]: zone localhost.localdomain/IN: loaded serial 0
Nov 17 16:12:28 dns.tp6.b1 named[1973]: all zones loaded
Nov 17 16:12:28 dns.tp6.b1 systemd[1]: Started Berkeley Internet Name Domain (DNS).
Nov 17 16:12:28 dns.tp6.b1 named[1973]: running
Nov 17 16:12:28 dns.tp6.b1 named[1973]: managed-keys-zone: Initializing automatic trust anchor management for zone '.'; DNSKEY ID 20326 is now >
Nov 17 16:12:28 dns.tp6.b1 named[1973]: resolver priming query complete
```

### Ecoute du port avec ss 

```
[aymeric@dns ~]$ ss -l -t -n | grep "10.6.1.101"
LISTEN 0      10        10.6.1.101:53        0.0.0.0:*
```

### On ouvre le bon port firewall

```PowerShell
[aymeric@dns ~]$ sudo firewall-cmd --add-port=53/tcp --permanent
[sudo] password for aymeric:
success
[aymeric@dns ~]$ sudo firewall-cmd --reload
success
```

## 3.Test

### 🌞 Sur la machine john.tp6.b1

```PowerShell
[aymeric@john ~]$ ping www.ynov.com
PING www.ynov.com (104.26.11.233) 56(84) bytes of data.
64 bytes from 104.26.11.233 (104.26.11.233): icmp_seq=1 ttl=55 time=20.4 ms
64 bytes from 104.26.11.233 (104.26.11.233): icmp_seq=2 ttl=55 time=19.8 ms
64 bytes from 104.26.11.233 (104.26.11.233): icmp_seq=3 ttl=55 time=20.5 ms
64 bytes from 104.26.11.233 (104.26.11.233): icmp_seq=4 ttl=55 time=20.6 ms
```

### 🌞 Sur votre PC

```PowerShell
PS C:\Users\aymer> ping jhon.tp6.b1

Envoi d’une requête 'ping' sur jhon.tp6.b1 [10.6.1.11] avec 32 octets de données :
Réponse de 10.6.1.11 : octets=32 temps<1ms TTL=64
Réponse de 10.6.1.11 : octets=32 temps<1ms TTL=64
Réponse de 10.6.1.11 : octets=32 temps=1 ms TTL=64
Réponse de 10.6.1.11 : octets=32 temps=1 ms TTL=64

Statistiques Ping pour 10.6.1.11:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 0ms, Maximum = 1ms, Moyenne = 0ms
```

### 🦈 Capture tp6_dns.pcapng

- [Voir la trame ici](./tp6_dns.pcapng)


## III. Serveur DHCP


### 🌞 Installer un serveur DHCP

> fichier conf

```PowerShell
[aymeric@dhcp ~]$ sudo cat /etc/dhcp/dhcpd.conf
[sudo] password for aymeric:
option domain-name     "srv.world";
option domain-name-servers     10.6.1.101;
default-lease-time 600;
max-lease-time 7200;
authoritative;
subnet 10.6.1.0 netmask 255.255.255.0 {
    range dynamic-bootp 10.6.1.13 10.6.1.37;
    option broadcast-address 10.0.0.255;
    option routers 10.6.1.254;
}
```

### 🌞 Test avec john.tp6.b1

- Ip dans la range

```PowerShell
[aymeric@john ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:68:f0:eb brd ff:ff:ff:ff:ff:ff
    inet 10.6.1.13/24 brd 10.0.0.255 scope global dynamic enp0s3
       valid_lft 477sec preferred_lft 477sec
    inet6 fe80::a00:27ff:fe68:f0eb/64 scope link
       valid_lft forever preferred_lft forever
```

- bon routeur par défault

```PowerShell
[aymeric@john ~]$ ip r s
default via 10.6.1.254 dev enp0s3
10.6.1.0/24 dev enp0s3 proto kernel scope link src 10.6.1.13
```

- Serveur DNS automatiquement configuré 

```PowerShell
[aymeric@john ~]$ sudo cat /etc/resolv.conf
[sudo] password for aymeric:
; generated by /usr/sbin/dhclient-script
search srv.world tp6.b1
nameserver 10.6.1.101
```

- Accés a Internet 

```PowerShell
[aymeric@john ~]$ ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=115 time=13.0 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=115 time=13.0 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=115 time=13.2 ms
```

### 🌞 Requête web avec john.tp6.b1

```PowerShell
[aymeric@john ~]$ curl www.ynov.com
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>302 Found</title>
</head><body>
<h1>Found</h1>
<p>The document has moved <a href="https://www.ynov.com/">here</a>.</p>
<hr>
<address>Apache/2.4.41 (Ubuntu) Server at ynov.com Port 80</address>
</body></html>
```
### 🦈 Capture tp6_web.pcapng

- [Voir la trame ici](./tp6_web.pcapng)