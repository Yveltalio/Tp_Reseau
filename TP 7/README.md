# Sizième TP 🐱

## 1. Fingerprint

## B. Manips

### 🌞 Effectuez une connexion SSH en vérifiant le fingerprint

- en rendu je veux voir le message du serveur à la première connexion

```PowerShell
PS C:\Users\aymer> ssh aymeric@jhon.tp7.b1
The authenticity of host 'jhon.tp7.b1 (10.7.1.11)' can't be established.
ED25519 key fingerprint is SHA256:0FIsart58Mlx1QwyXPXV48YXk6HdBkJlGK//wvRSO5w.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

- et je veux aussi une commande qui me montre l'empreinte du serveur

```PowerShell
[aymeric@john ~]$ sudo ssh-keygen -l -f /etc/ssh/ssh_host_ed25519_key
[sudo] password for aymeric:
256 SHA256:0FIsart58Mlx1QwyXPXV48YXk6HdBkJlGK//wvRSO5w /etc/ssh/ssh_host_ed25519_key.pub (ED25519)
```

## 2. Conf serveur SSH

### 🌞 Consulter l'état actuel

```PowerShell
[aymeric@john ~]$ ss -tlpn
State               Recv-Q              Send-Q                           Local Address:Port                           Peer Address:Port              Process
LISTEN              0                   128                                    0.0.0.0:22                                  0.0.0.0:*
LISTEN              0                   128                                       [::]:22                                     [::]:*
```

### 🌞 Modifier la configuration du serveur SSH

```PowerShell
[aymeric@routeur ~]$ ss -tlpn
State             Recv-Q            Send-Q                       Local Address:Port                         Peer Address:Port            Process
LISTEN            0                 128                             10.7.1.254:20001                             0.0.0.0:*
```

### 🌞 Effectuer une connexion SSH sur le nouveau port

```PowerShell
PS C:\Users\aymer> ssh aymeric@routeur.tp7.b1 -p 20001
aymeric@routeur.tp7.b1's password:
Last login: Thu Nov 23 15:59:12 2023 from 10.7.1.1
```

## 3. Connexion par clé

## A. Explications

### 🌞 Déposer la clé publique sur une VM

```PowerShell
 ssh-copy-id aymeric@john.tp7.b1
```

### 🌞 Connectez-vous en SSH à la machine

```PowerShell
PS C:\Users\aymer> ssh aymeric@jhon.tp7.b1
Last login: Thu Nov 23 16:11:29 2023 from 10.7.1.1
[aymeric@john ~]$
```

## C. Changement de fingerprint

### 🌞 Supprimer les clés sur la machine router.tp7.b1

```PowerShell
[aymeric@routeur ~]$ sudo rm /etc/ssh/ssh_host_*
[aymeric@routeur ~]$ sudo ls /etc/ssh/
moduli  ssh_config  ssh_config.d  sshd_config  sshd_config.d
```

### 🌞 Regénérez les clés sur la machine router.tp7.b1

```PowerShell
[aymeric@routeur ~]$ sudo ssh-keygen -A
ssh-keygen: generating new host keys: RSA DSA ECDSA ED25519
```

### 🌞 Montrer sur quel port est disponible le serveur web

```PowerShell
[aymeric@web ~]$ sudo ss -atlpn
State       Recv-Q      Send-Q           Local Address:Port            Peer Address:Port      Process
LISTEN      0           128                    0.0.0.0:22                   0.0.0.0:*          users:(("sshd",pid=680,fd=3))
LISTEN      0           511                    0.0.0.0:80                   0.0.0.0:*          users:(("nginx",pid=1815,fd=6),("nginx",pid=1814,fd=6))
LISTEN      0           128                       [::]:22                      [::]:*          users:(("sshd",pid=680,fd=4))
LISTEN      0           511                       [::]:80                      [::]:*          users:(("nginx",pid=1815,fd=7),("nginx",pid=1814,fd=7))
```

### 🌞 Générer une clé et un certificat sur web.tp7.b1

```PowerShell
[aymeric@web ~]$ openssl req -new -newkey rsa:2048 -days 365 -nodes -x509 -keyout server.key -out server.crt
.....+...+...+.+......+...+.....+.+.........+..+...+.+.....+.+.....+....+.....+.+.................+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*..+........+.............+.................+...+....+...+........+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*.....+......+.....+.+...............+..+...+...+.+...........+...+.+.....+....+.....................+...+......+.....+.+......+..+.+.........+......+.....+......+...+.+.....+....+.....+..........+......+............+.....+.......+..+..................+.......+...+......+.....+.......+.....+....+........+......+......+.+...+..+..........+.....+............+...+...+....+...+...+......+........+............+.+...+..+...............+...............+.............+...........+.........+.......+.........+......+..+......+......+.+...+.....+....+..+.+.......................+............+...................+.........+...........+...+......+......+......+..........+.........+...+...+............+..+.+.....+.........+.........+.+...+...+...+..+...+......+.........+.......+..+...+....+.....+.+..+.......+...+.................+...............+....+.....+.......+........+.+.................................+..+.........+....+..+.........+.......+...+.........+......+..+....+......+............+...+.........+.....+...+......+.+...+........+...+....+..+.....................+....+...+..+....+..........................+.+..+.+...........+.+...+......+........+...+...+.+......+.....+.......+.....+.......+...........+.......+...+.....+....+.....+................+.....+....+.....+......+...+.+.....+....+.....+......+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
..................+..+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*...+..+.+..+...+....+.....+.+...+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*.......+...+.........+.....+...+.......+...........+...+....+...........+.+.........+...........+.+..............+.............+.....+.+.....+......+.+...+...............+..+.......+.....+..........+...+.....+.......+.....+...+.........................+..+......+..........+............+..+.+..+...+.........+......+.+...+..+.+...........................+..+......+....+.........+..+.......+..+....+.....+.......+.........+........+......+......+.................................+..........+.....+.+.........+...........+.+.....+.+............+..+.+..............+....+..+...+....+..+.+...+.........+...........+....+...........+......+...+.............+...+..+...+....+..+......+...+....+..+................+..................+......+...+....................+...............+.+..+....+...+..+...+.......+...+...........+......+....+......+...+......+..+....+...+.....+............+.+..+.............+..+......+.+.....+.+..+..........+.....................+..+.............+.....+.+...........+....+...+...+...+............+..+.......+...+...+.....+....+.....+......+.......+.....+.......+........+...+....+........+...+.......+..+.+........+......+....+.....+.+...+...........+....+..+.........+....+......+..+...............+.............+..+....+...+..+.+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [XX]:
State or Province Name (full name) []:
Locality Name (eg, city) [Default City]:
Organization Name (eg, company) [Default Company Ltd]:
Organizational Unit Name (eg, section) []:
Common Name (eg, your name or your server's hostname) []:
Email Address []:
```

```PowerShell
[aymeric@web ~]$ sudo mv server.key /etc/pki/tls/private/web.tp7.b1.key
[aymeric@web ~]$ sudo mv server.crt /etc/pki/tls/certs/web.tp7.b1.crt
[aymeric@web ~]$ sudo chown nginx:nginx /etc/pki/tls/private/web.tp7.b1.key
[aymeric@web ~]$ sudo chown nginx:nginx /etc/pki/tls/certs/web.tp7.b1.crt
[aymeric@web ~]$ sudo chmod 0400 /etc/pki/tls/private/web.tp7.b1.key
[aymeric@web ~]$ sudo chmod 0444 /etc/pki/tls/certs/web.tp7.b1.crt
```

### 🌞 Modification de la conf de NGINX

```PowerShell
[aymeric@web ~]$ sudo cat /etc/nginx/conf.d/site_web_maxicool.conf
server {
  # le port sur lequel on veut écouter
  listen 10.7.1.12:443 ssl;

  # le nom de la page d'accueil si le client de la précise pas
  ssl_certificate /etc/pki/tls/certs/web.tp7.b1.crt;
  ssl_certificate_key /etc/pki/tls/private/web.tp7.b1.key;
  #index index.html;

  # un nom pour notre serveur (pas vraiment utile ici, mais bonne pratique)
  server_name www.site_web_maxicool.b1;

  # le dossier qui contient notre site web
  root /var/www/site_web_maxicool;
}
```

### 🌞 Prouvez que NGINX écoute sur le port 443/tcp

```PowerShell
[aymeric@web ~]$ sudo ss -atlpn
State       Recv-Q      Send-Q           Local Address:Port            Peer Address:Port      Process
LISTEN      0           128                    0.0.0.0:22                   0.0.0.0:*          users:(("sshd",pid=680,fd=3))
LISTEN      0           511                    0.0.0.0:80                   0.0.0.0:*          users:(("nginx",pid=2110,fd=7),("nginx",pid=2109,fd=7))
LISTEN      0           511                  10.7.1.12:443                  0.0.0.0:*          users:(("nginx",pid=2110,fd=6),("nginx",pid=2109,fd=6))
LISTEN      0           128                       [::]:22                      [::]:*          users:(("sshd",pid=680,fd=4))
LISTEN      0           511                       [::]:80                      [::]:*          users:(("nginx",pid=2110,fd=8),("nginx",pid=2109,fd=8))
```

## IV. VPN

### 3. On y va ou bien
 

```PowerShell
[aymeric@vpn ~]$ sudo modprobe wireguard
[aymeric@vpn ~]$ echo wireguard | sudo tee /etc/modules-load.d/wireguard.conf
wireguard
[aymeric@vpn ~]$ sudo nano /etc/sysctl.conf
[aymeric@vpn ~]$ sudo sysctl -p
net.ipv4.ip_forward = 1
net.ipv6.conf.all.forwarding = 1
```

### 🌞 Générer la paire de clé du serveur sur vpn.tp7.b1

```PowerShell
[aymeric@vpn ~]$ wg genkey | sudo tee /etc/wireguard/server.key
WJNpfwE5zr4YjuOTcZQd+wYx8HYTEYvaLnAF6Dg7MUQ=
[aymeric@vpn ~]$ sudo chmod 0400 /etc/wireguard/server.key
[aymeric@vpn ~]$ sudo cat /etc/wireguard/server.key | wg pubkey | sudo tee /etc/wireguard/server.pub
tlGeOwE4FEj6XPNqgoW6EZKiuAjTn3rFVLPigoU5OEg=
```

### 🌞 Générer la paire de clé du client sur vpn.tp7.b1

