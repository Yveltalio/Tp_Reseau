# Cinquième TP 🐱

## I. First steps

### 🌞Top 5 appli

> youtube
- UDP
- 172.217.20.206
- 443  
- [Voir la trame ici](./youtube.pcapng)
```PowerShell
PS C:\windows\system32> netstat  -n -a -b | Select-String oper -Context 1,0

    TCP    10.33.66.114:58799     64.233.167.188:5228    ESTABLISHED
>  [opera.exe]
    TCP    10.33.66.114:58913     107.167.110.216:443    CLOSE_WAIT
>  [opera.exe]
    TCP    10.33.66.114:58979     82.145.216.15:443      CLOSE_WAIT
>  [opera.exe]
    TCP    10.33.66.114:58980     107.167.110.216:443    CLOSE_WAIT
>  [opera.exe]
    TCP    10.33.66.114:58981     91.68.245.15:443       CLOSE_WAIT
>  [opera.exe]
    TCP    10.33.66.114:58982     185.26.182.112:443     ESTABLISHED
>  [opera.exe]
    TCP    10.33.66.114:58983     216.58.214.65:443      ESTABLISHED
>  [opera.exe]
    TCP    10.33.66.114:58984     142.250.179.98:443     ESTABLISHED
>  [opera.exe]

```
> spotify  
- UDP
- 35.186.224.25 : 443
- 51365
- [Voir la trame ici](./spotify.pcapng)
```PowerShell
PS C:\windows\system32> netstat  -n -a -b | Select-String Spo -Context 1,0

    TCP    0.0.0.0:49668          0.0.0.0:0              LISTENING
>  [spoolsv.exe]
    TCP    0.0.0.0:58953          0.0.0.0:0              LISTENING
>  [Spotify.exe]
    TCP    10.33.66.114:58954     104.199.65.124:4070    ESTABLISHED
>  [Spotify.exe]
    TCP    10.33.66.114:58956     35.186.224.25:443      ESTABLISHED
>  [Spotify.exe]
    TCP    10.33.66.114:58957     34.149.154.214:443     ESTABLISHED
>  [Spotify.exe]
    TCP    10.33.66.114:58958     34.149.154.214:443     ESTABLISHED
>  [Spotify.exe]
    TCP    10.33.66.114:58959     10.33.83.226:8009      SYN_SENT
>  [Spotify.exe]
    TCP    10.33.66.114:58960     142.250.201.161:443    ESTABLISHED
>  [Spotify.exe]
    TCP    10.33.66.114:58962     8.8.8.8:443            ESTABLISHED
>  [Spotify.exe]
    TCP    10.33.66.114:58963     162.159.61.3:443       ESTABLISHED
>  [Spotify.exe]
    TCP    10.33.66.114:58964     146.75.54.250:443      ESTABLISHED
>  [Spotify.exe]
    TCP    10.33.66.114:58965     142.250.179.98:443     ESTABLISHED

```

> discord  
- UDP
- 66.22.199.225 : 50007
- 51198
- [Voir la trame ici](./discord.pcapng)
```PowerShell
PS C:\windows\system32> netstat  -n -a -b | Select-String Disc -Context 1,0

    TCP    10.33.66.114:58914     162.159.135.232:443    ESTABLISHED
>  [Discord.exe]
    TCP    10.33.66.114:58916     162.159.136.234:443    ESTABLISHED
>  [Discord.exe]
    TCP    10.33.66.114:58921     8.8.4.4:443            ESTABLISHED
>  [Discord.exe]
    TCP    10.33.66.114:58922     172.64.41.3:443        ESTABLISHED
>  [Discord.exe]
    TCP    10.33.66.114:58923     8.8.4.4:443            ESTABLISHED
>  [Discord.exe]
    TCP    10.33.66.114:58933     162.159.129.233:443    ESTABLISHED
>  [Discord.exe]
    TCP    10.33.66.114:58945     162.159.137.234:443    ESTABLISHED
>  [Discord.exe]
    TCP    10.33.66.114:58946     162.159.134.232:443    ESTABLISHED
>  [Discord.exe]
    TCP    127.0.0.1:6463         0.0.0.0:0              LISTENING
```
> steam
- TCP
- 185.25.182.40 : 443
- 56491  
- [Voir la trame ici](./steam.pcapng)
```PowerShell
PS C:\windows\system32> netstat  -n -a -b | Select-String steam -Context 1,0

    TCP    0.0.0.0:27036          0.0.0.0:0              LISTENING
>  [Steam.exe]
    TCP    10.33.66.114:57770     51.140.202.63:443      ESTABLISHED
>  [msteams.exe]
    TCP    10.33.66.114:59095     23.54.132.227:443      ESTABLISHED
>  [Steam.exe]
    TCP    10.33.66.114:59099     185.25.182.20:27028    ESTABLISHED
>  [Steam.exe]
    TCP    10.33.66.114:59110     185.25.182.40:443      ESTABLISHED
>  [Steam.exe]
    TCP    10.33.66.114:59111     185.25.182.4:443       ESTABLISHED
>  [Steam.exe]
    TCP    10.33.66.114:59112     185.25.182.3:443       ESTABLISHED
>  [Steam.exe]
    TCP    10.33.66.114:59113     23.54.132.227:443      ESTABLISHED
>  [Steam.exe]
    TCP    10.33.66.114:59114     23.54.132.227:443      ESTABLISHED
>  [Steam.exe]
    TCP    10.33.66.114:59115     23.54.132.227:443      ESTABLISHED
>  [Steam.exe]
    TCP    10.33.66.114:59116     23.33.233.98:443       ESTABLISHED
>  [steamwebhelper.exe]
    TCP    10.33.66.114:59117     23.54.132.227:443      ESTABLISHED
>  [steamwebhelper.exe]
    TCP    10.33.66.114:59118     23.15.179.226:443      ESTABLISHED
>  [steamwebhelper.exe]
    TCP    10.33.66.114:59119     142.250.178.155:443    ESTABLISHED
>  [Steam.exe]

```
>  fnac
- UDP
- 104.26.9.51 : 443
- 63469
- [Voir la trame ici](./fnac.pcapng)
```PowerShell
PS C:\windows\system32> netstat  -n -a -b | Select-String oper -Context 1,0

    TCP    10.33.66.114:58799     64.233.167.188:5228    ESTABLISHED
>  [opera.exe]
    TCP    10.33.66.114:58913     107.167.110.216:443    CLOSE_WAIT
>  [opera.exe]
    TCP    10.33.66.114:58981     91.68.245.15:443       LAST_ACK
>  [opera.exe]
    TCP    10.33.66.114:58993     185.26.182.106:443     ESTABLISHED
>  [opera.exe]
    TCP    10.33.66.114:58994     172.217.20.174:443     ESTABLISHED
>  [opera.exe]
    TCP    10.33.66.114:58995     82.145.216.16:443      ESTABLISHED
>  [opera.exe]
    TCP    10.33.66.114:58996     96.16.248.137:443      ESTABLISHED
>  [opera.exe]
    TCP    10.33.66.114:58997     2.18.222.197:443       ESTABLISHED
>  [opera.exe]
    TCP    10.33.66.114:58998     104.18.130.236:443     ESTABLISHED
>  [opera.exe]
    TCP    10.33.66.114:58999     184.51.104.243:443     ESTABLISHED
>  [opera.exe]
    TCP    10.33.66.114:59000     95.217.77.225:443      ESTABLISHED

```

## II. Setup Virtuel

### 1. SSH

### 🌞 Examinez le trafic dans Wireshark

- [Voir la trame ici](./tp5_3_way.pcapng)


### 🌞 Demandez aux OS


```PowerShell
Last login: Fri Nov 10 14:27:52 2023 from 10.5.1.1
[aymeric@node5 ~]$ ss -t
State            Recv-Q            Send-Q                       Local Address:Port                         Peer Address:Port             Process
ESTAB            0                 52                               10.5.1.11:ssh                              10.5.1.1:54377
```

## 2. Routage

### 🌞 Prouvez que node5 a accès a internet

```PowerShell
[aymeric@node5 ~]$ ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=112 time=19.7 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=112 time=19.0 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=112 time=19.5 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=112 time=19.4 ms
```
### 🌞 Prouvez que node5 peut résoudre des noms de domaine publics
```PowerShell
[aymeric@node5 ~]$ ping google.com
PING google.com (172.217.18.206) 56(84) bytes of data.
64 bytes from ham02s14-in-f206.1e100.net (172.217.18.206): icmp_seq=1 ttl=115 time=19.6 ms
64 bytes from ham02s14-in-f206.1e100.net (172.217.18.206): icmp_seq=2 ttl=115 time=18.8 ms
64 bytes from ham02s14-in-f206.1e100.net (172.217.18.206): icmp_seq=3 ttl=115 time=18.9 ms
64 bytes from ham02s14-in-f206.1e100.net (172.217.18.206): icmp_seq=4 ttl=115 time=23.8 ms
64 bytes from ham02s14-in-f206.1e100.net (172.217.18.206): icmp_seq=5 ttl=115 time=18.3 ms
```

## 3. Serveur Web


### 🌞 On verifie la status après l'avoir lancé !
```PowerShell
[aymeric@web conf.d]$ systemctl status nginx
● nginx.service - The nginx HTTP and reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; preset: disabled)
     Active: active (running) since Fri 2023-11-10 16:13:31 CET; 10s ago
    Process: 11285 ExecStartPre=/usr/bin/rm -f /run/nginx.pid (code=exited, status=0/SUCCESS)
    Process: 11286 ExecStartPre=/usr/sbin/nginx -t (code=exited, status=0/SUCCESS)
    Process: 11287 ExecStart=/usr/sbin/nginx (code=exited, status=0/SUCCESS)
   Main PID: 11288 (nginx)
      Tasks: 2 (limit: 4674)
     Memory: 2.0M
        CPU: 25ms
     CGroup: /system.slice/nginx.service
             ├─11288 "nginx: master process /usr/sbin/nginx"
             └─11289 "nginx: worker process"

Nov 10 16:13:31 web systemd[1]: Starting The nginx HTTP and reverse proxy server...
Nov 10 16:13:31 web nginx[11286]: nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
Nov 10 16:13:31 web nginx[11286]: nginx: configuration file /etc/nginx/nginx.conf test is successful
Nov 10 16:13:31 web systemd[1]: Started The nginx HTTP and reverse proxy server.
```

### 🌞 Ouvrir le port firewall

```PowerShell
[aymeric@web conf.d]$sudo firewall-cmd --add-port=80/tcp --permanent
```

### 🌞 Analyse trafic


- [Voir la trame ici](./tp5_web.pcapng)