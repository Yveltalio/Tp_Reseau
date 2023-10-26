# Premier TP 🐱 

## I Exploration locale en solo  

## 1. Affichage d'informations sur la pile TCP/IP locale

## 🌞Affichez les infos des cartes réseau de votre PC

### Interface WiFi

```
> ipconfig all
```

>nom : Carte réseau sans fil WIFI  
adresse MAC : B4-8C-9D-D5-C6-63  
adresse IP : 10.33.48.33  

### Interface Ethernet

```
> ipconfig all
```

>nom : Carte Ethernet Ethernet  
adresse MAC : 7C-57-58-68-83-12  
adresse IP : none

## 🌞Affichez votre gateway

```
> ipconfig all
```

>adresse IP passerelle : 10.33.51.254

##  🌞Déterminer la MAC de la passerelle  

```
> arp -a
```

>adresse MAC de la passerelle :  7c-5a-1c-cb-fd-a4

## 🌞Trouvez comment afficher les informations sur une carte IP (change selon l'OS)

### ouvrir panneau de config > réseau internet > centre réseau partage > modifier les paramètre de partage > wifi > détail

>adresse ip : 10.33.48.33  
adresse MAC : B4-8C-9D-D5-C6-63  
adresse de la passerelle : 10.33.51.254

## 2. Modifications des informations

## 🌞Modification d'adresse IP

### Paramètre > Réseau et Internet > Wifi > Propriétés de WiFi@YNOV > Attribution de l'adresse IP

> J'ai perdu l'accès a internet car l'adresse ip etais deja attribué , récuperer l'accès a internet se fait en récuperant son adresse ip original ou en trouvant une libre

## III. Manipulations d'autres outils/protocoles côté client

## 1. DHCP

## 🌞Exploration du DHCP

```
> ipconfig -all
```
>10.33.51.254

## 2. DNS 

## 🌞Trouver l'adresse IP du serveur DNS que connaît votre ordinateur

```
> ipconfig /all
```

> 10.33.10.2

## 🌞Utiliser, en ligne de commande l'outil nslookup

### lookup

```
> nslookup google.com 8.8.8.8
```
> 142.250.179.110

```
> nslookup ynov.com 8.8.8.8
```
> 172.67.74.226  
104.26.11.233  
104.26.10.233

### reverse lookup

```
> nslookup 231.34.113.12 8.8.8.8
```

> dns.google ne parvient pas à trouver 231.34.113.12 : Non-existent domain

```
> nslookup 78.34.2.17 8.8.8.8
```

>cable-78-34-2-17.nc.de

## IV. Wireshark
