# Checkpoint-4-Questionnaire-Pro-Correction
# Questionnaire Professionnel - Checkpoint 4

---

## 💪 1. Assurer le support utilisateur en centre de service

### 1.1 Du point de vue d'ITIL, quelle est la différence entre un incident et un problème ?
- **Incident** : Interruption non planifiée (ou baisse de qualité) d'un service IT. Son objectif est de le restaurer au plus vite.
- **Problème** : Cause profonde (root cause) d'un ou plusieurs incidents, nécessitant une analyse approfondie pour éviter leur réapparition.

### 1.2 Quels sont les différents moyens de prendre le contrôle à distance d'une machine ?
- **RDP (Remote Desktop Protocol)** : Contrôle à distance natif Windows.
- **SSH (Secure Shell)** : Accès distant sécurisé, commun sur Linux/Unix.
- **VNC (Virtual Network Computing)** : Contrôle graphique multiplateforme.
- **Logiciels tiers** : TeamViewer, AnyDesk, Chrome Remote Desktop, etc.

### 1.3 Donne les différentes étapes à respecter dans une résolution d'incident par téléphone.
1. **Identification** de l'utilisateur et de son environnement.
2. **Compréhension** et reformulation du problème.
3. **Diagnostic** (questions ciblées, vérifications simples, prise de contrôle si besoin).
4. **Proposition** et mise en œuvre d'une solution.
5. **Validation** avec l'utilisateur (test final).
6. **Clôture** de l'incident (documentation du ticket).

---

## 🖥 2. Exploiter des serveurs Windows et un domaine Active Directory

### 2.1 Qu'est-ce qu'un rôle FSMO ?
Les **Flexible Single Master Operations (FSMO)** sont des rôles critiques assurant le bon fonctionnement d'Active Directory. Ils sont au nombre de 5 :
- **Schema Master** : Gère la cohérence et l'extension du schéma AD.
- **Domain Naming Master** : Gère l'ajout/suppression de domaines.
- **RID Master** : Gère l'attribution des identifiants uniques (RID) pour les objets.
- **PDC Emulator** : Fait office de contrôleur principal pour l'authentification, la synchronisation de l'heure et la rétrocompatibilité.
- **Infrastructure Master** : Gère les références croisées inter-domaines.

### 2.2 En quoi la réplication entre contrôleurs de domaine est primordiale sur un domaine ?
- **Cohérence des données** : Les modifications (créations de comptes, changements de mots de passe) sont répliquées.
- **Haute disponibilité** : En cas de panne d'un contrôleur, un autre prend le relais.
- **Performance** : Les requêtes sont traitées localement par chaque DC.

---

## 🐧 3. Exploiter des serveurs Linux

### 3.1 Quel est le résultat de la commande suivante : `chmod u+x /home/tssr/factures/export.sh` ?
Cela attribue le droit d'exécution au propriétaire (u) du fichier `export.sh`. Concrètement, l'utilisateur propriétaire peut désormais exécuter le script.

### 3.2 Quelle commande pour ajouter l'adresse IP 172.16.8.16/24 à l'interface enp0s8 ?
```bash
ip addr add 172.16.8.16/24 dev enp0s8
```

### 3.3 L'utilisateur Wilder ne parvient plus à accéder au dossier `travaux`.
- **Cause probable** : Problème de droits (permissions) ou de propriétaire sur le répertoire.
- **Outils pour diagnostiquer** :
  - `ls -ld /home/wilder/travaux` (affiche permissions et propriétaire)
  - `groups wilder` (affiche les groupes de l'utilisateur)
- **Commande pour résoudre** :
  - `sudo chown wilder:wilder /home/wilder/travaux` (reprise du propriétaire)
  - `sudo chmod 755 /home/wilder/travaux` (donne droits de lecture/exécution au groupe et autres si besoin)

### 3.4 Si on ajoute un disque dur supplémentaire qui n'a qu'une seule partition, comment se nommera-t-elle ?
Sous Linux, si le nouveau disque est détecté sous `/dev/sdb`, alors la partition unique sera **/dev/sdb1**.

### 3.5 Donne 2 commandes pour visualiser les disques et les partitions d'un serveur Linux.
- `lsblk` : Affiche l'arborescence des blocs (disques, partitions, volumes logiques).
- `fdisk -l` : Liste les disques et leurs partitions de façon plus détaillée.

---

## 🌍 4. Exploiter un réseau IP

### 4.1 Une entreprise a un réseau 192.160.16.0/25, souhaitant le découper en 4 sous-réseaux.
Pour les 2 premiers sous-réseaux :

| Sous-réseau | Adresse réseau    | CIDR  | Première adresse     | Dernière adresse      | Broadcast          |
|-------------|-------------------|-------|----------------------|-----------------------|--------------------|
| 1er         | 192.160.16.0      | /27   | 192.160.16.1         | 192.160.16.30         | 192.160.16.31     |
| 2e          | 192.160.16.32     | /27   | 192.160.16.33        | 192.160.16.62         | 192.160.16.63     |

### 4.2 Complète le tableau de conversion suivant :

| Décimal | Binaire    | Hexadécimal |
|---------|-----------|-------------|
| 9       | 00001001  | 0x09        |
| 127     | 01111111  | 0x7F        |
| 255     | 11111111  | 0xFF        |
| 16      | 00010000  | 0x10        |

### 4.3 Pour le schéma réseau : quels sont les liens trunk ? Quelle méthode de routage intervlan est utilisée ?
- **Liens trunk** : Généralement, ce sont les liens entre un switch et un routeur (pour du "router on a stick"), ou entre deux switches pour transporter plusieurs VLAN.
- **Méthode de routage intervlan** : Souvent un routeur sur une interface (router on a stick) ou un switch de niveau 3 (multilayer switching). Selon le schéma, si chaque VLAN est encapsulé sur un même lien, c'est du **router-on-a-stick**.

### 4.4 Sans changer l'adresse IP des 2 PC, donne une solution matérielle et une modification de paramétrage :
- **Solution matérielle** : Ajouter un routeur ou un firewall entre les deux réseaux (192.168.1.0/24 et 192.168.2.0/24).
- **Modification** : Créer des routes statiques ou activer le routage entre les deux réseaux.

### 4.5 Des ordinateurs sont connectés sur un switch (1 VLAN), IP ci-dessous :
| PC   | Adresse IP       | Masque           |
|------|------------------|------------------|
| PC1  | 192.168.10.8     | 255.255.255.0    |
| PC2  | 192.168.10.12    | 255.255.255.0    |
| PC3  | 192.168.10.10    | 255.255.0.0      |
| PC4  | 192.168.11.9     | 255.255.255.0    |

- **PC1 & PC2** : Communication ICMP réussie (même sous-réseau /24).
- **PC1 & PC3** : Réussie (PC3 considère 192.168.0.0/16, donc voit PC1 comme local).
- **PC2 & PC3** : Réussie pour la même raison.
- **PC4** : Échoue avec tous les autres (il est dans 192.168.11.0/24, différent sans route).

### 4.6 Quelles actions possibles pour sécuriser un réseau sans fil ?
- **Utiliser WPA2 ou WPA3** avec mot de passe robuste.
- **Filtrage MAC** (optionnel, mais peu fiable seul).
- **Réseau invité** séparé, VLAN dédié.
- **Désactiver WPS**.
- **Utiliser un portail captif** si nécessaire.

### 4.7 Quelles sont les routes statiques à ajouter sur Routeur1 pour permettre la communication entre PC0 et PC3 ?

Supposons :
- **Réseau PC0** : 192.168.0.0/24 côté LAN du Routeur1.
- **Réseau PC3** : 192.168.2.0/24 (par exemple) derrière un autre routeur.
- Sur Routeur1 :
```bash
ip route 192.168.2.0 255.255.255.0 <IP-de-saut-prochain>
```
- Et l'inverse sur le routeur côté PC3 pour rejoindre 192.168.0.0/24.

*(Les adresses exactes dépendent du schéma fourni.)*

### 4.8 Complète le tableau des services/protocoles :

| Acronyme | Nom complet                    | Ports TCP par défaut    | Ports UDP par défaut |
|----------|--------------------------------|-------------------------|----------------------|
| **HTTP** | HyperText Transfer Protocol    | 80                      | (N/A)                |
| **FTP**  | File Transfer Protocol         | 20 (données), 21 (cmd)  | (N/A)                |
| **SFTP** | SSH File Transfer Protocol     | 22 (via SSH)            | (N/A)                |
| **SSH**  | Secure Shell                   | 22                      | (N/A)                |
| **TFTP** | Trivial File Transfer Protocol | (N/A)                   | 69                   |
| **SMTP** | Simple Mail Transfer Protocol  | 25                      | (N/A)                |
| **IMAP** | Internet Message Access Prot.  | 143 (non sécu), 993 SSL | (N/A)                |
| **LDAP** | Lightweight Directory Access   | 389 (non sécu), 636 SSL | (N/A)                |
| **POP3** | Post Office Protocol           | 110 (non sécu), 995 SSL | (N/A)                |
| DNS      | Domain Name System             | 53                      | 53                   |
| **NTP**  | Network Time Protocol          | (N/A)                   | 123                  |

### 4.9 Sur quels ports du switch peut-on brancher ce téléphone IP ?
- En général, on branche un téléphone IP sur un **port configuré en accès** avec un VLAN Voix. Parfois un **port trunk** si le téléphone gère le VLAN data + VLAN voix. Le téléphone IP aura un VLAN voice dédié (802.1Q).

### 4.10 Indique la couche du modèle TCP/IP pour chaque protocole :

| Protocole | Accès Réseau | Internet | Transport | Application |
|-----------|-------------|----------|-----------|-------------|
| **ARP**   | X           |          |           |             |
| **Ethernet** | X         |          |           |             |
| **ICMP**  |             | X        |           |             |
| **IPv6**  |             | X        |           |             |
| **DHCP**  |             |          |           | X (application)  |
| **FTP**   |             |          |           | X (application)  |
| **TLS/SSL** |             |          |           | X (application)  |
| **POP3**  |             |          |           | X (application)  |
| **Telnet**|             |          |           | X (application)  |
| **SNMP**  |             |          |           | X (application)  |

(En simplifiant le modèle TCP/IP à 4 couches, la couche Application englobe session/présentation/application du modèle OSI.)

---

## 🚀 5. Maintenir des serveurs dans une infrastructure virtualisée

### 5.1 Qu'est-ce qu'un cluster d'hyperviseur ? Quel est l’intérêt ?
- Un **cluster d’hyperviseurs** regroupe plusieurs hôtes (serveurs physiques) exécutant des hyperviseurs. Il offre :
  - **Haute disponibilité** : Si un hôte tombe, les VM peuvent être migrées.
  - **Répartition de charge** : Distribution dynamique des ressources.

### 5.2 Qu'est-ce qu'un container ? Donne différentes solutions.
- Un **container** est un environnement isolé contenant les dépendances nécessaires à l’exécution d’une application.
- Exemples : **Docker**, **LXC**, **Podman**, **Kubernetes** (orchestrateur).

### 5.3 Que représentent ces lignes de code (Dockerfile) ? Comment les utiliser ?
```dockerfile
FROM ubuntu:latest

# Installation de packages
RUN apt-get update && apt-get install -y \
    bash \
    nano \
    && rm -rf /var/lib/apt/lists/*

# Répertoire local
RUN mkdir /data

# Dossier de travail
WORKDIR /data

# Image en mode interactif
CMD ["bash", "-i"]
```
- **Explication** :
  - Part de l’image de base Ubuntu.
  - Installe bash, nano.
  - Crée le dossier `/data` et le définit comme répertoire de travail.
  - Lance la commande bash en mode interactif.
- **Utilisation** :
  - Placer ce Dockerfile dans un dossier.
  - `docker build -t monimage .`
  - `docker run -it monimage`

### 5.4 Pour la copie d'écran (Nagios, alerte critique sur swap=0%) :
1. **Vérifier** l’état du swap via `free -m`.
2. **Activer ou augmenter** le swap si nécessaire (fichier swap ou partition).
3. **Vérifier** les processus gourmands en mémoire (`top`, `htop`).
4. **Configurer** une alerte plus adaptée si 0% correspond à swap désactivé.

### 5.5 Que veulent dire PaaS, IaaS, et SaaS ?
- **PaaS (Platform as a Service)** : Fournit une plateforme complète (runtime, DB, etc.) pour le développement/déploiement d’applications.
- **IaaS (Infrastructure as a Service)** : Fournit des machines virtuelles, stockage, réseaux (AWS EC2, Azure VM...).
- **SaaS (Software as a Service)** : Fournit des applications déjà hébergées (ex: Office 365, Gmail...).

### 5.6 Dans la mise en oeuvre d'une solution HA, quels sont les éléments indispensables ?
- **Redondance** (plusieurs nœuds / serveurs).
- **Load Balancer** ou mécanisme de répartition.
- **Stockage partagé** (NAS / SAN) ou réplication.
- **Supervision** et mécanismes de bascule automatique (failover).

---

## 🔐 6. Maintenir et sécuriser les accès à Internet et les interconnexions des réseaux

### 6.1 Quel est l'impact des ACL ci-dessous sur la machine 172.16.0.10 ? Peut-on fusionner ces ACL ?
```
access-list 100 deny icmp host 172.16.0.10 172.17.0.0 0.255.255.255
access-list 100 permit ip any any

access-list 101 deny tcp host 172.16.0.10 host 220.0.0.60 eq www
access-list 101 deny tcp host 172.16.0.10 host 220.0.0.60 eq 443
access-list 101 permit ip any any
```
- **Impact** :
  - ACL 100 : Interdit le ping (ICMP) depuis 172.16.0.10 vers 172.17.x.x.
  - ACL 101 : Interdit l’accès TCP (port 80 et 443) depuis 172.16.0.10 vers 220.0.0.60.
- **Fusion** possible en une seule ACL :
```
access-list 110 deny icmp host 172.16.0.10 172.17.0.0 0.255.255.255
access-list 110 deny tcp host 172.16.0.10 host 220.0.0.60 eq 80
access-list 110 deny tcp host 172.16.0.10 host 220.0.0.60 eq 443
access-list 110 permit ip any any
```

### 6.2 Pourquoi, malgré 2 chaînes différentes, le résultat sha512sum a la même longueur ?
Parce que la **taille du résumé (hash)** produit par SHA-512 est **fixe** (512 bits), quel que soit la taille du message en entrée.

### 6.3 Sur l'infrastructure réseau, que faut-il faire pour accéder de manière sécurisée au serveur web depuis internet ?
- **Mettre en place** une redirection (NAT) sur le pare-feu vers la DMZ.
- **Ouvrir** les ports nécessaires (HTTPS 443, HTTP 80 si besoin) sur le firewall.
- **Ajouter** un certificat SSL/TLS sur le serveur web pour le chiffrement.

### 6.4 Quels types de VPN sont représentés dans les illustrations ?
- **VPN A (nomade)** : VPN "Remote Access" ou "Client-to-site".
- **VPN B (site à site)** : VPN reliant deux réseaux distants.

### 6.5 Complète le texte :
> Pour envoyer un message privé à Bob, Alice utilise **(expression 1)** de Bob pour rendre « illisible » le « texte en clair » et Bob utilise **(expression 2)** pour transformer le texte « illisible » en « texte en clair ». Ce processus représente un chiffrement **(expression 3)**.

- **expression 1** : la clé publique de Bob
- **expression 2** : la clé privée de Bob
- **expression 3** : un chiffrement asymétrique

### 6.6 Traduction du passage ISAKMP en français :
> "Lorsque les négociations ISAKMP commencent, le pair initiateur envoie toutes ses politiques au pair distant, et ce dernier essaie de trouver une correspondance. Il compare chacune des politiques de l'initiateur avec celles qu'il a configurées, dans l'ordre de priorité (de la plus haute à la plus basse), jusqu'à trouver une correspondance."

### 6.7 Indique 3 types de menaces (risques/attaques) possibles :
- **Ransomware** (chiffrement de données avec rançon)
- **Phishing** (hameçonnage par mail ou site frauduleux)
- **Attaques DDoS** (déni de service distribué)
- *(autres exemples possibles : virus, vol de données, etc.)*

---

## 💾 7. Mettre en place, assurer et tester les sauvegardes et restaurations

### 7.1 Qu'est-ce que la règle 3-2-1 ?
- **3** copies des données au total
- **2** supports de stockage différents
- **1** copie hors site (externe)

### 7.2 Quels sont les différents types de sauvegardes à mettre en place ?
- **Sauvegarde complète** (Full) : Copie de toutes les données.
- **Sauvegarde différentielle** : Copie des modifications depuis la dernière sauvegarde complète.
- **Sauvegarde incrémentale** : Copie des modifications depuis la dernière sauvegarde (complète ou incrémentale).

### 7.3 Différences entre sauvegarde, archivage et clonage
- **Sauvegarde** : Recopie des données pour restauration rapide en cas de sinistre.
- **Archivage** : Conservation à long terme de données (légales/historiques), souvent peu modifiées.
- **Clonage** : Réplication exacte d'un système (ex: un disque ou système d'exploitation) pour déploiement ou remplacement.

---

## 🖨 8. Exploiter et maintenir les services de déploiement des postes de travail

### 8.1 Quels avantages apporte un service centralisé de mises à jour logicielles ? Une solution ?
- **Avantages** :
  - Contrôle et planification des mises à jour.
  - Réduction de la bande passante (les mises à jour sont téléchargées une fois).
  - Homogénéité : toutes les machines sont mises à jour de la même manière.
- **Exemple de solution** : **WSUS** (Windows Server Update Services) sous Windows.
  - Fonctionnement : Le serveur WSUS récupère les mises à jour de Microsoft, puis les distribue aux postes clients configurés pour aller chercher leurs mises à jour auprès de ce serveur.

### 8.2 Inconvénients d’une solution de terminaux clients légers vs postes fixes
- **Dépendance au réseau** : Si la connexion au serveur est défaillante, les clients ne peuvent pas travailler.
- **Performances limitées** : Tout est traité côté serveur, qui doit être dimensionné.
- **Personnalisation plus complexe** : Les utilisateurs partagent un environnement serveur.

### 8.3 Que fait la commande suivante ? Dans quel contexte est-elle utilisée ?
```bash
C:\Windows\System32\sysprep\sysprep.exe /oobe /generalize /shutdown
```
- **Action** : Sysprep généralise l’image Windows, en supprimant les identifiants uniques (SID), afin de préparer le système à un déploiement. Le PC s’éteint après ce processus.
- **Contexte** : Utilisé lors de la création d’une image master pour le déploiement de plusieurs postes.

### 8.4 Dans le cadre d'un déploiement de postes clients Linux, quelle est l'utilité d'un dépôt local de paquet ?
- **Utilité** : Éviter de télécharger chaque paquet depuis Internet pour chaque machine. Réduit la consommation de bande passante, accélère les installations et permet de contrôler précisément les versions de paquets déployées.

---

## 🧐 Critères d'acceptation
Les réponses doivent être fournies dans le formulaire réponse et transmises au format PDF. Chaque question a été abordée dans ce document pour couvrir l’ensemble du questionnaire professionnel.

---


