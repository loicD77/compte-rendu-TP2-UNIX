# [ LP LPW 2024 ] compte rendu TP2 UNIX :
DARRAS Loïc L3 PRO PROJET WEB ET MOBILE


# Présentation/Introduction du travail (avant la table des matières et les explications)


* Ici ce deuxième tp UNIX concene les services et les processus signaux.
* J'ai tout d'abord effectué une connexion ssh root, une authentification par clef (une génération de clefs, une connexion serveur, depuis la machine hôte) avec des explications sur la cybersécurié avec les attaques de type brute-force ssh et des méthodes pour lutter contre ceci
* Puis je me suis concentré sur les processus, sur l'arrèt de ces processus et enfin les tubes et le journal système rsyslog.




# I) Secure Shell : SSH

1.1 . Comme le demande le tp, je me suis connecté en root et j'ai configuré le ssh . Pour cela j'ai utilisé les commandes **apt search** et **apt install** . (Ici ce sont des exmples fictifs avec WSL avec Windows car je n'ai pas enregisté ceci au moment des faits)


* apt install

```bash
No manual entry for sshd_config
root@LAPTOP-E9LS6Q7M:/mnt/c/Windows/System32# sudo apt install openssh-server
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  libwrap0 ncurses-term openssh-sftp-server ssh-import-id
Suggested packages:
  molly-guard monkeysphere ssh-askpass
The following NEW packages will be installed:
  libwrap0 ncurses-term openssh-server openssh-sftp-server ssh-import-id
0 upgraded, 5 newly installed, 0 to remove and 0 not upgraded.
Need to get 799 kB of archives.
After this operation, 6157 kB of additional disk space will be used.
Do you want to continue? [Y/n] Y
Get:1 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 openssh-sftp-server amd64 1:8.9p1-3ubuntu0.10 [38.9 kB]
Get:2 http://archive.ubuntu.com/ubuntu jammy/main amd64 libwrap0 amd64 7.6.q-31build2 [47.9 kB]
Get:3 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 openssh-server amd64 1:8.9p1-3ubuntu0.10 [435 kB]
Get:4 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 ncurses-term all 6.3-2ubuntu0.1 [267 kB]
Get:5 http://archive.ubuntu.com/ubuntu jammy/main amd64 ssh-import-id all 5.11-0ubuntu1 [10.1 kB]
Fetched 799 kB in 2s (325 kB/s)
Preconfiguring packages ...
Selecting previously unselected package openssh-sftp-server.
(Reading database ... 26221 files and directories currently installed.)
Preparing to unpack .../openssh-sftp-server_1%3a8.9p1-3ubuntu0.10_amd64.deb ...
Unpacking openssh-sftp-server (1:8.9p1-3ubuntu0.10) ...
Selecting previously unselected package libwrap0:amd64.
Preparing to unpack .../libwrap0_7.6.q-31build2_amd64.deb ...
Unpacking libwrap0:amd64 (7.6.q-31build2) ...
Selecting previously unselected package openssh-server.
Preparing to unpack .../openssh-server_1%3a8.9p1-3ubuntu0.10_amd64.deb ...
Unpacking openssh-server (1:8.9p1-3ubuntu0.10) ...
Selecting previously unselected package ncurses-term.
Preparing to unpack .../ncurses-term_6.3-2ubuntu0.1_all.deb ...
Unpacking ncurses-term (6.3-2ubuntu0.1) ...
Selecting previously unselected package ssh-import-id.
Preparing to unpack .../ssh-import-id_5.11-0ubuntu1_all.deb ...
Unpacking ssh-import-id (5.11-0ubuntu1) ...
Setting up openssh-sftp-server (1:8.9p1-3ubuntu0.10) ...
Setting up ssh-import-id (5.11-0ubuntu1) ...
Setting up libwrap0:amd64 (7.6.q-31build2) ...
Setting up ncurses-term (6.3-2ubuntu0.1) ...
Setting up openssh-server (1:8.9p1-3ubuntu0.10) ...

Creating config file /etc/ssh/sshd_config with new version
Creating SSH2 RSA key; this may take some time ...
3072 SHA256:tm+znTuTeKjENfuVO1X4EuuIj7Xpg7CQvXvaqK+hS94 root@LAPTOP-E9LS6Q7M (RSA)
Creating SSH2 ECDSA key; this may take some time ...
256 SHA256:jIVcxJ/2FU6SvdAzagDmosVj4Iai+/qobohLNsDM3KQ root@LAPTOP-E9LS6Q7M (ECDSA)
Creating SSH2 ED25519 key; this may take some time ...
256 SHA256:ne35cQ2UeZYEpLLCHZDyDqjP8xzLE4MavncH+3tbwEA root@LAPTOP-E9LS6Q7M (ED25519)
Created symlink /etc/systemd/system/sshd.service → /lib/systemd/system/ssh.service.
Created symlink /etc/systemd/system/multi-user.target.wants/ssh.service → /lib/systemd/system/ssh.service.
rescue-ssh.target is a disabled or a static unit, not starting it.
ssh.socket is a disabled or a static unit, not starting it.
Processing triggers for ufw (0.36.1-4ubuntu0.1) ...
Processing triggers for man-db (2.10.2-1) ...
Processing triggers for libc-bin (2.35-0ubuntu3.8) ...
root@LAPTOP-E9LS6Q7M:/mnt/c/Windows/System32#
```


* apt search 


```bash
root@LAPTOP-E9LS6Q7M:/mnt/c/Windows/System32# apt search openssh
Sorting... Done
Full Text Search... Done
agent-transfer/jammy 0.43-3.1 amd64
  copy a secret key from GnuPG's gpg-agent to OpenSSH's ssh-agent

backuppc/jammy 4.4.0-5ubuntu2 amd64
  high-performance, enterprise-grade system for backing up PCs

cme/jammy 1.037-1 all
  Check or edit configuration data with Config::Model

connect-proxy/jammy 1.105-1.2 amd64
  Establish TCP connection using SOCKS4/5 or HTTP tunnel

crypto-policies/jammy 20190816git-1 all
  unify the crypto policies used by different applications and libraries

debian-goodies/jammy-updates,jammy-security 0.87ubuntu1.1 all
  Small toolbox-style utilities for Debian systems

diffoscope/jammy 205 all
  in-depth visual diff tool for files, archives and directories

diffoscope-minimal/jammy 205 all
  in-depth visual diff tool for files, archives and directories (minimal package)

fwknop-apparmor-profile/jammy 2.6.10-13build1 all
  FireWall KNock OPerator - Apparmor profile

fwknop-client/jammy 2.6.10-13build1 amd64
  FireWall KNock OPerator client side - C version

fwknop-server/jammy 2.6.10-13build1 amd64
  FireWall KNock OPerator server side - C version

gesftpserver/jammy 1~ds-3 amd64
  sftp server submodule for OpenSSH

golang-github-calmh-randomart-dev/jammy 1.1.0-2 all
  generates OpenSSH-style randomart (Go library)

golang-github-mikesmitty-edkey-dev/jammy 0.0~git20170222.3356ea4-1 all
  generates ED25519 private keys in the OpenSSH private key format (Go library)

keychain/jammy 2.8.5-2 all
  key manager for OpenSSH

kwalletcli/jammy 3.03-1 amd64
  command line interface to the KDE Wallet

lacme/jammy 0.8.0-2 all
  ACME client written with process isolation and minimal privileges in mind

lacme-accountd/jammy 0.8.0-2 all
  lacme account key manager

libconfig-model-cursesui-perl/jammy 1.107-1 all
  curses interface to edit config data through Config::Model

libconfig-model-openssh-perl/jammy 2.8.7.1-1 all
  configuration editor for OpenSsh

libconfig-model-tkui-perl/jammy 1.375-1 all
  Tk GUI to edit config data through Config::Model

libfko-doc/jammy 2.6.10-13build1 all
  FireWall KNock OPerator - documentation

libfko-perl/jammy 2.6.10-13build1 amd64
  FireWall KNock OPerator - Perl module

libfko3/jammy 2.6.10-13build1 amd64
  FireWall KNock OPerator - shared library

libfko3-dev/jammy 2.6.10-13build1 amd64
  FireWall KNock OPerator - development library

libjsch-agent-proxy-java/jammy 0.0.9-1 all
  Proxy to ssh-agent and Pageant in Java

libnet-openssh-compat-perl/jammy 0.09-1 all
  collection of compatibility modules for Net::OpenSSH

libnet-openssh-parallel-perl/jammy 0.12-1.1 all
  run SSH jobs in parallel

libnet-openssh-perl/jammy 0.80-1 all
  Perl SSH client package implemented on top of OpenSSH

libnet-sftp-foreign-perl/jammy 1.93+dfsg-1 all
  client for the Secure File Transfer Protocol

libnet-sftp-sftpserver-perl/jammy 1.1.0-6 all
  Secure File Transfer Protocol Server

libtrilead-putty-extension-java/jammy 1.2-2 all
  PuTTY key support for Trilead SSH2 library

login-duo/jammy 1.11.3-1build1 amd64
  login wrapper for Duo Security two-factor authentication

lxqt-openssh-askpass/jammy 0.17.0-0ubuntu1 amd64
  OpenSSH user/password GUI dialog for LXQt

lxqt-openssh-askpass-l10n/jammy 0.17.0-0ubuntu1 all
  Language package for lxqt-openssh-askpass

monkeysphere/jammy 0.43-3.1 all
  leverage the OpenPGP web of trust for SSH and TLS authentication

mysecureshell/jammy 2.0-2build1 amd64
  SFTP Server with ACL

node-sshpk/jammy 1.16.1+dfsg1-1 all
  library for finding and using SSH public keys

openssh-client/jammy-updates,jammy-security,now 1:8.9p1-3ubuntu0.10 amd64 [installed,automatic]
  secure shell (SSH) client, for secure access to remote machines

openssh-client-ssh1/jammy 1:7.5p1-13 amd64
  secure shell (SSH) client for legacy SSH1 protocol

openssh-known-hosts/jammy 0.6.2-1.1 all
  download, filter and merge known_hosts for OpenSSH

openssh-server/jammy-updates,jammy-security,now 1:8.9p1-3ubuntu0.10 amd64 [installed]
  secure shell (SSH) server, for secure access from remote machines

openssh-sftp-server/jammy-updates,jammy-security,now 1:8.9p1-3ubuntu0.10 amd64 [installed,automatic]
  secure shell (SSH) sftp server module, for SFTP access from remote machines

openssh-tests/jammy-updates,jammy-security 1:8.9p1-3ubuntu0.10 amd64
  OpenSSH regression tests

putty-tools/jammy 0.76-2 amd64
  command-line tools for SSH, SCP, and SFTP

python3-asyncssh/jammy 2.5.0-1 all
  asyncio-based client and server implementation of SSHv2 protocol

python3-scp/jammy 0.13.0-2 all
  scp module for paramiko (Python 3)

python3-setproctitle/jammy 1.2.2-2build1 amd64
  Setproctitle implementation for Python 3

python3-sshpubkeys/jammy 3.1.0-2.1 all
  SSH public key parser - Python 3

secpanel/jammy 1:0.6.1-3 all
  graphical user interface for SSH and SCP

ssh/jammy-updates,jammy-security 1:8.9p1-3ubuntu0.10 all
  secure shell client and server (metapackage)

ssh-askpass-gnome/jammy-updates,jammy-security 1:8.9p1-3ubuntu0.10 amd64
  interactive X program to prompt users for a passphrase for ssh-add

ssh-audit/jammy 2.5.0-1 all
  tool for ssh server auditing

sshpass/jammy 1.09-1 amd64
  Non-interactive ssh password authentication

```









* J'ai ensuite utilisé le **man sshd_config** dont voici le résultat : 

```bash
SSHD_CONFIG(5)                                 BSD File Formats Manual                                SSHD_CONFIG(5)

NAME
     sshd_config — OpenSSH daemon configuration file

DESCRIPTION
     sshd(8) reads configuration data from /etc/ssh/sshd_config (or the file specified with -f on the command line).
     The file contains keyword-argument pairs, one per line.  For each keyword, the first obtained value will be
     used.  Lines starting with ‘#’ and empty lines are interpreted as comments.  Arguments may optionally be en‐
     closed in double quotes (") in order to represent arguments containing spaces.

     Note that the Debian openssh-server package sets several options as standard in /etc/ssh/sshd_config which are
     not the default in sshd(8):

           •   Include /etc/ssh/sshd_config.d/*.conf
           •   KbdInteractiveAuthentication no
           •   X11Forwarding yes
           •   PrintMotd no
           •   AcceptEnv LANG LC_*
           •   Subsystem sftp /usr/lib/openssh/sftp-server
           •   UsePAM yes

     /etc/ssh/sshd_config.d/*.conf files are included at the start of the configuration file, so options set there
     will override those in /etc/ssh/sshd_config.

     The possible keywords and their meanings are as follows (note that keywords are case-insensitive and arguments
     are case-sensitive):

     AcceptEnv
 Manual page sshd_config(5) line 1 (press h for help or q to quit)

```


1.2 .  J'ai créé  une clef d’authentification pour me connecter directement a mon serveur Linux.:

```bash
1013781@ppti-24-308-17:~$ ssh-keygen
Generating public/private ed25519 key pair.
Enter file in which to save the key (/users/Etu1/1013781/.ssh/id_ed25519): /users/Etu1/1013781/.ssh/id_rsa
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /users/Etu1/1013781/.ssh/id_rsa
Your public key has been saved in /users/Etu1/1013781/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:EE4ER2qgZAv9DRyy7Ad8tVQWdjjXebgTfmH9k67JBG8 1013781@ppti-24-308-17
The key's randomart image is:
+--[ED25519 256]--+
|.+o.o=O.=o.. o . |
|++o+oB =o.. = + .|
|..=.+o+  o . = .o|
| . +. ..   .+ .o.|
|  . .   S   oo. .|
|   .         E . |
|            + o  |
|             +   |
|                 |
+----[SHA256]-----+

```


1.3  Ici j'ai copié la clé publique sur le serveur :

```bash 
ssh-copy-id root@10.20.0.188 
```

Je me suis connecté au serveur sans avoir besoin d'entrer de mot de passe avec l'aide de la commande suivante : 

```bash 
1013781@ppti-24-308-17:/users/nfs/Etu1/1013781$ ssh root@10.20.0.188
root@10.20.0.188's password:
Linux serveur-correction 6.1.0-12-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.52-1 (2023-09-07) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Wed Oct  9 15:38:10 2024
root@serveur-correction
```



J'ai bien généré une clé dans le dossser ".ssh" : 


```bash 
013781@ppti-24-308-17:~/.ssh$ ls -la
total 16
drwx------  2 1013781 1013781 4096 11 oct.  15:56 .
drwxr-xr-x 17 1013781 1013781 4096 11 oct.  14:04 ..
-rw-------  1 1013781 1013781  419 11 oct.  15:56 id_rsa
-rw-r--r--  1 1013781 1013781  104 11 oct.  15:56 id_rsa.pub
-rw-------  1 1013781 1013781    0 11 oct.  14:16 known_hosts
-rw-------  1 1013781 1013781    0 11 oct.  14:16 known_hosts.old
1013781@ppti-24-308-17:~/.ssh$ ssh-copy-id root@serveur-correction
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed

/usr/bin/ssh-copy-id: ERROR: ssh: Could not resolve hostname serveur-correction: Name or service not known

1013781@ppti-24-308-17:~/.ssh$ ssh-copy-id root@10.20.0.188
The authenticity of host '10.20.0.188 (10.20.0.188)' can't be established.
ED25519 key fingerprint is SHA256:ZnqDicCb1/9DRN6O1faHyKbNW0S/OzwMwCumkD9mkXk.


```


1.4 . J'ai bien utilisé la commande **ssh -i maclef.pub root@ipserveur**


```bash 
1013781@ppti-24-308-17:~/.ssh$ ssh-copy-id -i id_rsa.pub root@10.20.0.188
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed

/usr/bin/ssh-copy-id: WARNING: All keys were skipped because they already exist on the remote system.
(if you think this is a mistake, you may want to use -f option)

```



1.5 . 
 
Source : Diamond Connect 

* Les attaques de type brute-force ssh sont des essaies (tentatives) de connexions SSH effectuant une succession d'essaies pour découvrir un couple utilisateur/mot de passe valide afin de prendre le contrôle de la machine .
* Il s'agit d'une attaque populaire et les machines victimes de celle-ci exposée sur Internet se verra attaquer en conséquence plusieurs fois par jour .
* La fréquence régulière de ces attaques a provoqué l'apparition de diverses méthodes de protection telle que :
    * l'utilisation d'un port non standard pour le serveur SSH. (Cela réduit la visibilité de votre service SSH aux attaquants, mais ne garantit pas une protection totale. C’est une solution simple et rapide qui augmente la sécurité sans effort significatif, particulièrement utile pour éviter les attaques opportunistes.)
    * l'utilisation d'outils comme **Fail2ban** qui analyse les journaux SSH pour trouver les adresses IP responsables de trop nombreux échecs de connexions et modifie la configuration du pare-feu pour bloquer les tentatives ultérieures. (Je l'ai choisi car cet outil analyse les journaux du serveur pour détecter les tentatives de connexion échouées répétées et bloque temporairement l’adresse IP de l’attaquant via le pare-feu. C'est donc une solution efficace pour automatiser la gestion des tentatives d’intrusion. C’est un bon complément à d'autres solutions comme la restriction d'accès par clé.)
* Encore d'autres méthodes  : 
    * Interdire les connexions SSH de l'utilisateur root à distance (selon it-connect.fr). En effet, il peut être risqué de laisser l'utilisateur rooy se **loguer** en SSH car cela peut ouvrir la porte à des brutes force qui donneront un accès direct au plus haut niveau de privilège de la machine. Il faut mettre dans ce cas **PermitRootLogin no** en décommentant et redémarrer le service SSH avec **service ssh restart** (Interdire les connexions SSH de root à distance réduit le risque d'attaques brute-force, car root est une cible privilégiée pour les pirates. Cela force l'utilisation de comptes utilisateurs standards avec sudo, ajoutant une couche de sécurité et de contrôle. Cette méthode limite aussi les erreurs critiques et encourage les meilleures pratiques en matière de gestion des privilèges administratifs.)
    * Limiter les accèes SSH par adresse IP.  Notamment avec TCP Wrapper. On peut par exemple refuser tous les accès dans le fichier **/etc/hosts.deny** : **sshd: ALL** et n'accepter dans le fichier /etc/hosts.allow que les connexions depuis des adresses IP validées : **sshd: 192.168.1.  221.10.140.10** (Je l'ai choisi car c’est une méthode très sécurisée, surtout dans les environnements où l’on peut prédéfinir les IP autorisées. Cela empêche toute tentative d'accès depuis des sources inconnues.)
    * On peut aussi sécuriser les connexions SSH par le parefeu. Un des meilleurs firewalls pour Linux est IPtables (associé à Netfilter) selon "https://www.alsacreations.com/tuto/lire/622-securite-firewall-iptables.html". Voici un script de configuration possible dans son cas.
         
         
   1) Utiliser la commande  **iptables -L -v**  pour lister les règles en place (3 chaînes : INPUT, FORWARD et OUTPUT).

   2) On va  créer un script qui sera lancé à chaque démarrage pour mettre en place des règles de base:

```bash 
vi /etc/init.d/firewall
```

  3) On active le filtrage et on redémarre, ou on  exécute **/etc/init.d/firewall** (L’utilisation d’un pare-feu est une méthode de base pour toute configuration serveur, et elle permet de protéger SSH contre des attaques en bloquant les adresses IP non autorisées. C’est une mesure de protection essentielle pour contrôler les connexions au serveur.)
      * Utiliser l'authentication à 2 facteurs (2FA) sur SSH (openclassroom) permet de réduire significativement les risques de compromission des comptes, pour ceci il faut **modifier le sshd_config et activer les méthodes d'authentication désirées.** ( Le 2FA est une solution incontournable pour les systèmes sensibles. Même si elle demande plus de gestion, elle apporte un niveau de sécurité bien supérieur aux méthodes traditionnelles.)
 


II ) Processus

2.1 .Exercice : Etude des processus UNIX


1.


* Voici un exemple d'utilisation de la commande ps :


```bash 
This message is shown once a day. To disable it please create the
/root/.hushlogin file.
root@LAPTOP-E9LS6Q7M:/mnt/c/Windows/System32# ps
    PID TTY          TIME CMD
    420 pts/0    00:00:00 bash
    523 pts/0    00:00:00 ps
root@LAPTOP-E9LS6Q7M:/mnt/c/Windows/System32#
```


* Voici l'explication de l'information TIME :


```bash 
ime        TIME      Temps CPU cumulé, au format « [JJ-]HH:MM:SS » (alias cputime).
```




```bash 
E9LS6Q7M:/mnt/c/Windows/System32# top
top - 11:24:59 up 8 min,  1 user,  load average: 0.03, 0.04, 0.00
Tasks:  42 total,   1 running,  41 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.1 us,  0.1 sy,  0.0 ni, 99.7 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :   7602.3 total,   6619.2 free,    572.7 used,    410.5 buff/cache
MiB Swap:   2048.0 total,   2048.0 free,      0.0 used.   6788.4 avail Mem

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
    312 root      20   0  154368  71104  17900 S   0.3   0.9   0:05.99 python3.10
      2 root      20   0    2616   1440   1320 S   0.0   0.0   0:00.01 init-systemd(Ub
      6 root      20   0    2616      4      0 S   0.0   0.0   0:00.00 init
     45 root      19  -1   31348  12108  11092 S   0.0   0.2   0:00.26 systemd-journal
     68 root      20   0   21968   5852   4488 S   0.0   0.1   0:00.38 systemd-udevd
     78 root      20   0  152992    184     24 S   0.0   0.0   0:00.00 snapfuse
     82 root      20   0  152992   2232     24 S   0.0   0.0   0:00.00 snapfuse
     87 root      20   0  377284  11216    212 S   0.0   0.1   0:01.09 snapfuse
     92 root      20   0  153124   2252     48 S   0.0   0.0   0:00.00 snapfuse
    101 root      20   0  152992    208     44 S   0.0   0.0   0:00.00 snapfuse
    106 root      20   0  302520   6828    312 S   0.0   0.1   0:00.24 snapfuse
    111 root      20   0  152992   2216     24 S   0.0   0.0   0:00.00 snapfuse
    115 root      20   0  302520  12844    196 S   0.0   0.2   0:02.16 snapfuse
    124 systemd+  20   0   25540  12640   8444 S   0.0   0.2   0:00.50 systemd-resolve
    141 root      20   0    4308   2704   2468 S   0.0   0.0   0:00.01 cron
    147 message+  20   0    8544   4584   3984 S   0.0   0.1   0:00.10 dbus-daemon
    169 root      20   0   30104  19080  10300 S   0.0   0.2   0:00.31 networkd-dispat
    170 syslog    20   0  222404   7088   4276 S   0.0   0.1   0:00.04 rsyslogd
    172 root      20   0 1984832  43396  18360 S   0.0   0.6   0:01.16 snapd
    198 root      20   0   15340   7428   6472 S   0.0   0.1   0:00.31 systemd-logind
    267 root      20   0    4784   3324   3084 S   0.0   0.0   0:00.15 subiquity-serve
    273 root      20   0  107228  21464  13208 S   0.0   0.3   0:00.18 unattended-upgr
    281 root      20   0    3240   1056    972 S   0.0   0.0   0:00.00 agetty 
```



* D'après les résultats de la commande top, le processus qui utilise le plus de CPU est effectivement python3.10, avec une utilisation de 0.3%. Voici un résumé :



```bash 
PID : 312
Utilisateur : root
Utilisation du CPU : 0.3%
Mémoire utilisée : 71 104 Ko
```

* Le premier processus lancée après le déemarrage du système :



```bash 
root@LAPTOP-E9LS6Q7M:/mnt/c/Windows/System32# ps -p 1
    PID TTY          TIME CMD
      1 ?        00:00:12 systemd
root@LAPTOP-E9LS6Q7M:/mnt/c/Windows/System32#

```

* Le premier processus lancé après le démarrage de mon système est **systemd**


* Heure 


```bash 
root@LAPTOP-E9LS6Q7M:/mnt/c/Windows/System32# who -b
         system boot  2024-10-13 11:16
root@LAPTOP-E9LS6Q7M:/mnt/c/Windows/System32#
```


* La sortie indique que mon système a démarré le 13 octobre 2024 à 11:16

Voici une autre commande : 

```bash 
root@LAPTOP-E9LS6Q7M:/mnt/c/Windows/System32# uptime
 11:39:06 up 23 min,  1 user,  load average: 0.06, 0.04, 0.00
root@LAPTOP-E9LS6Q7M:/mnt/c/Windows/System32#
```


* up 23 min : Cela indique que ma machine est en fonctionnement depuis 23 minutes.


```bash 
 root@LAPTOP-E9LS6Q7M:/mnt/c/Windows/System32# ps -e | wc -l
44
root@LAPTOP-E9LS6Q7M:/mnt/c/Windows/System32#
```

Il y a donc environ 44 processus actifs sur ma machine depuis son démarrage.




2.

Trouver une option de la commande ps permettant d’afficher le PPID d’un processus :

ps -ef


```bash 
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  1 11:16 ?        00:00:29 /sbin/init
root           2       1  0 11:16 ?        00:00:00 /init
root           6       2  0 11:16 ?        00:00:00 plan9 --control-socket 6 --log-level 4 --server-fd 7 --pipe-fd 9 --l
root          45       1  0 11:16 ?        00:00:00 /lib/systemd/systemd-journald
root          68       1  0 11:16 ?        00:00:00 /lib/systemd/systemd-udevd
root          78       1  0 11:16 ?        00:00:00 snapfuse /var/lib/snapd/snaps/bare_5.snap /snap/bare/5 -o ro,nodev,a
root          82       1  0 11:16 ?        00:00:00 snapfuse /var/lib/snapd/snaps/core22_1380.snap /snap/core22/1380 -o
root          87       1  0 11:16 ?        00:00:01 snapfuse /var/lib/snapd/snaps/core22_1621.snap /snap/core22/1621 -o
root          92       1  0 11:16 ?        00:00:00 snapfuse /var/lib/snapd/snaps/gtk-common-themes_1535.snap /snap/gtk-
root         101       1  0 11:16 ?        00:00:00 snapfuse /var/lib/snapd/snaps/snapd_20290.snap /snap/snapd/20290 -o
root         106       1  0 11:16 ?        00:00:00 snapfuse /var/lib/snapd/snaps/snapd_21759.snap /snap/snapd/21759 -o
root         111       1  0 11:16 ?        00:00:00 snapfuse /var/lib/snapd/snaps/ubuntu-desktop-installer_1276.snap /sn
root         115       1  0 11:16 ?        00:00:02 snapfuse /var/lib/snapd/snaps/ubuntu-desktop-installer_1286.snap /sn
systemd+     124       1  0 11:16 ?        00:00:00 /lib/systemd/systemd-resolved
root         141       1  0 11:16 ?        00:00:00 /usr/sbin/cron -f -P
message+     147       1  0 11:16 ?        00:00:00 @dbus-daemon --system --address=systemd: --nofork --nopidfile --syst
root         169       1  0 11:16 ?        00:00:00 /usr/bin/python3 /usr/bin/networkd-dispatcher --run-startup-triggers
syslog       170       1  0 11:16 ?        00:00:00 /usr/sbin/rsyslogd -n -iNONE
root         172       1  0 11:16 ?        00:00:01 /usr/lib/snapd/snapd
root         198       1  0 11:16 ?        00:00:00 /lib/systemd/systemd-logind
root         267       1  0 11:16 ?        00:00:00 /bin/bash /snap/ubuntu-desktop-installer/1286/bin/subiquity-server
root         273       1  0 11:16 ?        00:00:00 /usr/bin/python3 /usr/share/unattended-upgrades/unattended-upgrade-s
root         281       1  0 11:16 hvc0     00:00:00 /sbin/agetty -o -p -- \u --noclear --keep-baud console 115200,38400,
root         296       1  0 11:16 tty1     00:00:00 /sbin/agetty -o -p -- \u --noclear tty1 linux
root         303       1  0 11:16 ?        00:00:00 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups
root         312     267  0 11:16 ?        00:00:08 /snap/ubuntu-desktop-installer/1286/usr/bin/python3.10 -m subiquity.
postgres     321       1  0 11:16 ?        00:00:00 /usr/lib/postgresql/14/bin/postgres -D /var/lib/postgresql/14/main -
postgres     349     321  0 11:16 ?        00:00:00 postgres: 14/main: checkpointer
postgres     350     321  0 11:16 ?        00:00:00 postgres: 14/main: background writer
postgres     351     321  0 11:16 ?        00:00:00 postgres: 14/main: walwriter
postgres     352     321  0 11:16 ?        00:00:00 postgres: 14/main: autovacuum launcher
postgres     353     321  0 11:16 ?        00:00:00 postgres: 14/main: stats collector
postgres     354     321  0 11:16 ?        00:00:00 postgres: 14/main: logical replication launcher
root         418       2  0 11:16 ?        00:00:00 /init
root         419     418  0 11:16 ?        00:00:00 /init
root         420     419  0 11:16 pts/0    00:00:00 -bash
root         421       2  0 11:16 pts/1    00:00:00 /bin/login -f
root         472       1  0 11:16 ?        00:00:00 /lib/systemd/systemd --user
root         477     472  0 11:16 ?        00:00:00 (sd-pam)
root         494     421  0 11:16 pts/1    00:00:00 -bash
root         509     312  0 11:16 ?        00:00:15 python3 /snap/ubuntu-desktop-installer/1286/usr/bin/cloud-init statu
root        9803     420  0 11:55 pts/0    00:00:00 ps -ef
root@LAPTOP-E9LS6Q7M:/mnt/c/Windows/System32#
```


* Donner la liste ordonnée de tous les processus ancêtres de la commande ps en cours d’exécution.




```bash 
root@LAPTOP-E9LS6Q7M:/mnt/c/Windows/System32# pstree -p 8946
root@LAPTOP-E9LS6Q7M:/mnt/c/Windows/System32#
```



* Cela peut ne rien retourner si ps n'a pas d'enfants.















