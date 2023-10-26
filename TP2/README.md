# Deuxième TP 🐱 

## I. Setup IP

### 🌞ip choisi :

>10.10.10.33/33  
10.10.10.32/34

>adresse de réseau : 10.10.10.32  
adresse de broadcast : 10.10.10.35

## 🌞On change l'ip et le masque de notre carte ethernet 

```powershell
PS C:\Users\aymer>New-NetIPAddress –InterfaceIndex 23 –IPAddress 10.10.10.34 –PrefixLength 30  

IPAddress         : 10.10.10.34
InterfaceIndex    : 23
InterfaceAlias    : Ethernet
AddressFamily     : IPv4
Type              : Unicast
PrefixLength      : 30
PrefixOrigin      : Manual
SuffixOrigin      : Manual
AddressState      : Tentative
ValidLifetime     :
PreferredLifetime :
SkipAsSource      : False
PolicyStore       : ActiveStore

IPAddress         : 10.10.10.34
InterfaceIndex    : 23
InterfaceAlias    : Ethernet
AddressFamily     : IPv4
Type              : Unicast
PrefixLength      : 30
PrefixOrigin      : Manual
SuffixOrigin      : Manual
AddressState      : Invalid
ValidLifetime     :
PreferredLifetime :
SkipAsSource      : False
PolicyStore       : PersistentStore
```

## 🌞On ping notre binome

```Powershell
PS C:\windows\system32> ping 10.10.10.33  

Envoi d’une requête 'Ping'  10.10.10.33 avec 32 octets de données :
Réponse de 10.10.10.33 : octets=32 temps=3 ms TTL=128
Réponse de 10.10.10.33 : octets=32 temps=3 ms TTL=128
Réponse de 10.10.10.33 : octets=32 temps=3 ms TTL=128
Réponse de 10.10.10.33 : octets=32 temps=3 ms TTL=128

Statistiques Ping pour 10.10.10.33:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 3ms, Maximum = 3ms, Moyenne = 3ms
```

## 🦈Trame Ethernet du ping avec WireShark

[Voir fichier ici](./packet1.pcapng)

## 2. ARP my bro

### 🌞Mon ARP

```powershell
PS C:\windows\system32> arp -a

Interface : 10.33.48.32 --- 0x4
  Adresse Internet      Adresse physique      Type
  10.33.48.14           be-be-52-62-83-b7     dynamique
  10.33.48.82           e4-b3-18-48-36-68     dynamique
  10.33.51.254          7c-5a-1c-cb-fd-a4     dynamique
  10.33.51.255          ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  255.255.255.255       ff-ff-ff-ff-ff-ff     statique

Interface : 192.168.247.1 --- 0xa
  Adresse Internet      Adresse physique      Type
  192.168.247.254       00-50-56-f6-fa-a6     dynamique
  192.168.247.255       ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  255.255.255.255       ff-ff-ff-ff-ff-ff     statique

Interface : 10.102.1.1 --- 0xc
  Adresse Internet      Adresse physique      Type
  10.102.1.255          ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 10.10.10.34 --- 0x17
  Adresse Internet      Adresse physique      Type
  10.10.10.33           50-eb-f6-30-83-17     dynamique
  10.10.10.35           ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  255.255.255.255       ff-ff-ff-ff-ff-ff     statique
```

## 🌞ARP du binom

```powershell
PS C:\windows\system32> arp -a 10.10.10.33

Interface : 10.10.10.34 --- 0x17
  Adresse Internet      Adresse physique      Type
  10.10.10.33           50-eb-f6-30-83-17     dynamique
```

## 🌞ARP de la passerelle Ynov

```powershell
PS C:\windows\system32> arp -a 10.33.51.254

Interface : 10.33.48.32 --- 0x4
  Adresse Internet      Adresse physique      Type
  10.33.51.254          7c-5a-1c-cb-fd-a4     dynamique
```

## 🌞On vide la table ARP

```powershell
PS C:\windows\system32> netsh interface IP delete arpcache
Ok.
```

## 🌞On affiche la table ARP après l'avoir vidé

```powershell
PS C:\windows\system32> arp -a

Interface : 10.33.48.32 --- 0x4
  Adresse Internet      Adresse physique      Type
  10.33.51.254          7c-5a-1c-cb-fd-a4     dynamique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 192.168.247.1 --- 0xa
  Adresse Internet      Adresse physique      Type
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 10.102.1.1 --- 0xc
  Adresse Internet      Adresse physique      Type
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 10.10.10.34 --- 0x17
  Adresse Internet      Adresse physique      Type
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

```

## 🌞On affiche la table ARP après un ping du binome

```powershell
PS C:\windows\system32> arp -a

Interface : 10.33.48.32 --- 0x4
  Adresse Internet      Adresse physique      Type
  10.33.51.254          7c-5a-1c-cb-fd-a4     dynamique
  10.33.51.255          ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 192.168.247.1 --- 0xa
  Adresse Internet      Adresse physique      Type
  192.168.247.255       ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 10.102.1.1 --- 0xc
  Adresse Internet      Adresse physique      Type
  10.102.1.255          ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 10.10.10.34 --- 0x17
  Adresse Internet      Adresse physique      Type
  10.10.10.33           50-eb-f6-30-83-17     dynamique
  10.10.10.35           ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  ```

## 🦈Trames Ethernet du "premier" ping avec WireShark

[Voir fichier ici](./packet2.pcapng)