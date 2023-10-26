# Troisième TP 🐱

## 1.Echange ARP

### 🌞Générer des requêtes ARP

```PowerShell
[aymeric@localhost ~]$ ping 10.3.1.11
PING 10.3.1.11 (10.3.1.11) 56(84) bytes of data.
64 bytes from 10.3.1.11: icmp_seq=1 ttl=64 time=4.11 ms
64 bytes from 10.3.1.11: icmp_seq=2 ttl=64 time=1.45 ms
64 bytes from 10.3.1.11: icmp_seq=3 ttl=64 time=0.986 ms
64 bytes from 10.3.1.11: icmp_seq=4 ttl=64 time=1.84 ms
```
### VM qui a envoyé le ping
```PowerShell
[aymeric@localhost ~]$ ip neigh show
10.3.1.11 dev enp0s3 lladdr 08:00:27:1a:c3:c5 STALE
10.3.1.1 dev enp0s3 lladdr 0a:00:27:00:00:50 REACHABLE
```

### VM qui a recu le ping

```PowerShell
[aymeric@localhost ~]$ ip neigh show
10.3.1.1 dev enp0s3 lladdr 0a:00:27:00:00:50 DELAY
10.3.1.12 dev enp0s3 lladdr 08:00:27:e8:80:ea STALE
```

### Adresse Mac de marcel depuis Jhon :
```PowerShell
[aymeric@localhost ~]$ ip neigh show 10.3.1.12
10.3.1.12 dev enp0s3 lladdr 08:00:27:e8:80:ea STALE
```

### Adresse Mac de marcel depuis marcel
```PowerShell
[aymeric@localhost ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:e8:80:ea brd ff:ff:ff:ff:ff:ff
    inet 10.3.1.12/24 brd 10.3.1.255 scope global noprefixroute enp0s3
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fee8:80ea/64 scope link
       valid_lft forever preferred_lft forever
```
### Adresse mac : 08:00:27:e8:80:ea

## 2.Analyse des trames

### On vide les tables arp 
 ```PowerShell
 [aymeric@localhost ~]$ sudo ip neigh flush all
 ```
 ### Ping marcel a Jhon

 ```PowerShell
 [aymeric@localhost ~]$ ping 10.3.1.11
 ```

[Voir la trame correspondante](./tp3_arp.pcapng)

## II.Routage

### 1.Mise en place du routage

### Route de réseau du réseau de Marcel au routeur coté Jhon

```PowerShell
[aymeric@localhost ~]$ sudo ip route add 10.3.1.0/24 via 10.3.2.254 dev enp0s3
```

### Route de réseau du réseau de Jhon au routeur coté Marcel
```PowerShell
[aymeric@localhost ~]$ sudo ip route add 10.3.2.0/24 via 10.3.1.254 dev enp0s3
```


### Ping Marcel depuis Jhon 

```PowerShell
[aymeric@localhost ~]$ ping 10.3.2.12
PING 10.3.2.12 (10.3.2.12) 56(84) bytes of data.
64 bytes from 10.3.2.12: icmp_seq=1 ttl=63 time=3.22 ms
64 bytes from 10.3.2.12: icmp_seq=2 ttl=63 time=2.65 ms
64 bytes from 10.3.2.12: icmp_seq=3 ttl=63 time=2.53 ms
^C
--- 10.3.2.12 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2006ms
rtt min/avg/max/mdev = 2.528/2.797/3.216/0.299 ms
```

## 2.Analyse des trames

### On vide nos tables ARP

```PowerShell
[aymeric@localhost ~]$sudo ip neigh flush all
```

### On effectue un ping de jhon vers marcel 

```PowerShell
[aymeric@localhost ~]$ ping 10.3.2.12
PING 10.3.2.12 (10.3.2.12) 56(84) bytes of data.
64 bytes from 10.3.2.12: icmp_seq=1 ttl=63 time=5.76 ms
64 bytes from 10.3.2.12: icmp_seq=2 ttl=63 time=3.88 ms
64 bytes from 10.3.2.12: icmp_seq=3 ttl=63 time=2.94 ms
64 bytes from 10.3.2.12: icmp_seq=4 ttl=63 time=2.58 ms
```
 
 ### Tableau


| ordre | type trame  | IP source | MAC source                  | IP destination | MAC destination             |
| ----- | ----------- | --------- | -------------------------   | -------------- | --------------------------  |
| 1     | Requête ARP | 10.3.1.11 | `jhon` `08:00:27:1a:c3:c5`  | 10.3.2.12      | Broadcast `FF:FF:FF:FF:FF`  |
| 2     | Réponse ARP | 10.3.2.12 | `marcel` `08:00:27:e8:80:ea`| 10.3.1.11      | `jhon` `08:00:27:1a:c3:c5`  |
| ...   | ...         | ...       | ...                         |                |                             |
| ?     | Ping        | 10.3.1.11 | `jhon` `08:00:27:1a:c3:c5`  | 10.3.2.12      | `marcel` `08:00:27:e8:80:ea`|
| ?     | Pong        | 10.3.2.12 | `marcel` `08:00:27:e8:80:ea`| 10.3.1.11      | `jhon` `08:00:27:1a:c3:c5`  |

## Accés a Internet 

### Après avoir rajouter une carte NAT a notre routeur :

```PowerShell
[aymeric@localhost ~]$ ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=114 time=18.6 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=114 time=27.0 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=114 time=21.1 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=114 time=22.0 ms
```

### On donne accés a Internet a nos machines 

### On met en route par défaut le routeur chez Jhon

```PowerShell
[aymeric@localhost ~]$ sudo ip route add default via 10.3.1.254 dev enp0s3
```

### On fait pareil chez Marcel

```PowerShell
[aymeric@localhost ~]$ sudo ip route add default via 10.3.2.254 dev enp0s3
```
