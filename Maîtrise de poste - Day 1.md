# Maîtrise de poste - Day 1

## Self-footprinting

### Host OS

> La commande `neofetch` nous donne toutes les informations nécessaires : 
* Host: 81EU Lenovo ideapad 530S-14IKB
* OS et Version: ArcoLinuxD 5.4.38-1-lts
* Architecture: x86_64
* Intel i5-8250U (8) @ 3.400GHz
* 8192 MB de RAM DDR4

### Devices

:sun_with_face:

> Processeur: 
* Marque: Intel
* Modèle: i5-8250U
```
Explication du nom:

Les Core i5 sont des processeurs puissants en dual ou quad core capables
d’atteindre de bonnes fréquences.

Le nom du processeur est suivi par une série de 4 chiffres dont le premier
est la génération de processeur (ici la 8ème).

Les trois chiffres suivants désignent la référence du processeur.
Le plus important est le premier de ces trois chiffres, plus il est élevé,
plus le processeur est haut de gamme,
enfin le U désigne un processeurs Intel Core mobiles bicœurs
basse consommation que l’on trouve principalement dans des Ultrabooks
```

> Touchpad:
 * MSFT0001:01 04F3:304B Touchpad

> Enceintes intégrées:
 * Intel Sunrise Point-LP HD Audio

> Disque Dur Principal:
 * SK hynix Disk
 * Modèle HFS512GD9TNG-62A0A

:sun_with_face:

> La commande `lsblk` nous donne les informations suivantes : 
```
 NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
nvme0n1     259:0    0   477G  0 disk 
├─nvme0n1p1 259:1    0   300M  0 part /boot/efi
├─nvme0n1p2 259:2    0 467,9G  0 part /
└─nvme0n1p3 259:3    0   8,8G  0 part [SWAP]
```
> Ici on peut observer que le nom de mon disque dur est `nvme0n1` et qu'il est partitionné en 3 parties : 
 * /boot/efi - Qui est la partition sur laquelle boot l'ordinateur lorsqu'il est sur un systeme UEFI.
 * / - Est la partition où est installée la racine, c'est ici que sont installés, utilisateurs, programmes, etc.
 * [SWAP] - Elle fait office de tampon, si toute la RAM est utilisée, alors les programmes seront ouverts dans cette partition plutôt que sur la RAM.
 
 ### Network
 
 :sun_with_face:  Afficher la liste des cartes réseau de votre machine
 
 > La commande `ip a` nous donne les infos suivantes : 
 ```
 1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: wlp1s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 64:5d:86:6b:22:45 brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.83/24 brd 192.168.1.255 scope global dynamic noprefixroute wlp1s0
       valid_lft 77776sec preferred_lft 77776sec
    inet6 fe80::e143:7d12:e35b:b043/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever

 ```
  * lo : C'est la loopback, présente sur tout PC et utilisée à des fins de tests.
  * wlp1s0 : C'est ma carte wifi, parce que WireLess.

:sun_with_face:  Lister tous les ports TCP et UDP en utilisation

> La commande `netstat -tup` donne les infos suivantes : 
```
Active Internet connections (w/o servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp       32      0 here:36382              ec2-52-209-86-177:https CLOSE_WAIT  88049/vivaldi-bin - 
tcp        0      0 here:54724              ec2-54-92-108-83.:https ESTABLISHED 88049/vivaldi-bin - 
tcp        0      0 here:34574              162.159.133.233:https   ESTABLISHED 90602/Discord --typ 
tcp        0      0 here:44242              lb-140-82-113-26-:https ESTABLISHED 88049/vivaldi-bin - 
tcp       32      0 here:35914              ec2-54-171-29-225:https CLOSE_WAIT  88049/vivaldi-bin - 
tcp        0      0 here:50392              ec2-54-92-108-83.:https ESTABLISHED 88049/vivaldi-bin - 
tcp        0      0 here:50128              162.159.133.234:https   ESTABLISHED 90602/Discord --typ 
tcp        0      0 here:49110              47.224.186.35.bc.:https ESTABLISHED 90602/Discord --typ 
udp        0      0 here:bootpc             box:bootps              ESTABLISHED 410/NetworkManager
```
> On peut donc voir, que les programmes en écoute sont Vivaldi (un navigateur web), Discord (un client de chat) et mon NetworkManager.

### Users

> On peut facilement déterminer les utilisateurs sur une machine linux, il suffit d'utiliser la commande `echo $(cut -d: -f1 /etc/passwd)\r`, elle liste tous les utilisateurs. L'administrateur système ayant tous les droits est le compte *Root* sur linux.

### Processus

> La commande `ps aux` retourne les process en cours d'utilisation, parmi ceux-ci, on peut sortir du lot : 
 * /sbin/init, c'est le premier process à ce lancer, s'execute jusqu'a ce que la machine soit éteinte, parent de tous les autres process.
 * /usr/bin/NetworkManager qui me permet de manager mes connexions inernet, sans ça pas de co
 * /usr/bin/lightdm qui est mon display manager, si je n'avais pas ça ou un autre display manager, je ne pourrais pas me connecter a ma session graphiquement.
 * /usr/lib/systemd qui permet de gérer quels services sont actifs ou non, permet notemment l'utilisation de la commande `systemctl`
 * i3 qui est mon window manager, sans lui pas de contrôle sur mes fenêtres, donc compliquer de travailler.

> Pour déterminer les processus lancés par l'utilisateur full admin, on peut utiliser `ps -xU root`.

### Scripting

> Voici le script permettant de print les infos de la machine

```
#!/bin/bash
 
#################################
# Auteur: Paul-Alexis Verrier   # 
# Date : 08/05/2020             # 
#                               # 
#                               # 
#                               # 
# Description : Ce script       # 
# permet de récuprer les infos  #
# de la machine où il est lancé #
#################################
 

os=$(uname -o)
pc_name=$(uname -n)
os_version=$(uname -sr)
ip_add=$(ip a | grep dynamic | awk '{print $2}')
upt=$(uptime -s)
uram=$(free -m | grep Mem | awk 'NR==1{print $3}')
fram=$(free -m | grep Mem | awk 'NR==1{print $4}')
udisk=$(df -BM | tail -6 | awk 'NR==1{print $3}')
fdisk=$(df -BM | tail -6 | awk 'NR==1{print $4}')
users=$(cut -d: -f1 /etc/passwd)\r
ping=$(ping -c 1 8.8.8.8 | grep avg | awk 'NR==1{print $4}' | cut -d '/' -f 2)
dspeed=$(curl -s https://raw.githubusercontent.com/sivel/speedtest-cli/master/speedtest.py | python - | tail -3 | awk 'NR==1{print $2,$3}')
uspeed=$(curl -s https://raw.githubusercontent.com/sivel/speedtest-cli/master/speedtest.py | python - | tail -1 | awk 'NR==1{print $2,$3}')

echo -n "Nom de l'ordinateur : " $pc_name
echo  ''
echo -n "Système d'exploitation : " $os
echo  ''
echo -n "Version : " $os_version
echo  ''
echo -n "Adresse IP : " $ip_add
echo  ''
echo  "Date et heure d'allumage : " $upt
echo  ''
echo -n "Besoin de mise à jour ? "
echo  "Pas trouvé de moyen pour le faire"
echo  ''
echo -n "RAM utilisée : " $uram Mo
echo  ''
echo  "RAM libre : " $fram Mo
echo  ''
echo  "Espace utilisé : " $udisk
echo  "Espace libre : " $fdisk
echo  ''
echo  "Utilisateurs sur le PC : " $users
echo  ''
echo  "Ping vers google en : " $ping ms
echo  ''
echo  "Vitesse de download : " $dspeed
echo  "Vitesse d'upload : " $uspeed
```

> Et son output

```
Nom de l'ordinateur :  here
Système d'exploitation :  GNU/Linux
Version :  Linux 5.4.38-1-lts
Adresse IP :  192.168.1.83/24
Date et heure d'allumage :  2020-05-09 13:50:02

Besoin de mise à jour ? Pas trouvé de moyen pour le faire

RAM utilisée :  1893 Mo
RAM libre :  3967 Mo

Espace utilisé :  31030M
Espace libre :  415527M

Utilisateurs sur le PC :  root bin daemon mail ftp http nobody dbus systemd-journal-remote systemd-network systemd-resolve systemd-timesync systemd-coredump uuidd nbd avahi colord dnsmasq git lightdm nm-openconnect nm-openvpn ntp partimag polkitd rpc usbmux paulex rtkit cups mysqlr

Ping vers google en :  10.884 ms

Vitesse de download :  269.50 Mbit/s
Vitesse d'upload :  288.94 Mbit/s
```