Voici un guide pas-à-pas pour réaliser les différentes interventions demandées dans la mise en situation professionnelle.  
L’objectif est de te permettre de comprendre **quoi faire** et **comment le démontrer** (copies d’écran) afin de compléter le fichier de réponse.  
Les explications sont rédigées de manière à te guider dans les manipulations.

---

## 1. Adaptation de script

### Contexte
- Machine concernée : **CLIENT01** (Windows 10)
- Chemin du script initial : `C:\Scripts\GetIpConfiguration.ps1`
- Deux nouvelles copies du script à créer :
  - `GetIpConfiguration_1.ps1`
  - `GetIpConfiguration_2.ps1`
- Fichiers d’export : 
  - `export1.csv`
  - `export2.csv`  
  (ils doivent contenir les informations séparées par des points-virgules `;`)

### Intervention 1

> **Tâche**  
> Dans le script `GetIpConfiguration.ps1`, tu vois des commentaires du type `# XXX`.  
> Remplace chaque `# XXX` par une phrase expliquant ce que fait la ou les lignes qui suivent.

1. Ouvre **PowerShell ISE** ou **Visual Studio Code** (ou même le Bloc-notes si besoin) en **exécutant en tant qu’administrateur**.  
2. Parcours le script `C:\Scripts\GetIpConfiguration.ps1`.  
3. À chaque `# XXX`, remplace par un commentaire explicite. Exemple :  
   ```powershell
   # Récupère la configuration réseau de tous les adaptateurs et stocke le résultat dans la variable $nets
   $nets = Get-NetIPAddress
   ```
4. **Sauvegarde** le script modifié.

#### Capture(s) d’écran attendue(s) (Question 1)
- Le script **affiché** montrant tes nouveaux commentaires.  
  (Fais une capture complète ou plusieurs captures partielles, mais de manière à voir toutes les lignes avec leurs nouveaux commentaires.)

---

### Intervention 2

> **Tâche**  
> Dupliquer le script en deux versions et produire deux affichages (et deux CSV) différents.

#### 2.1. Script `GetIpConfiguration_1.ps1`

- **Objectif d’affichage en console** (exactement dans cet ordre de colonnes) :
  ```
  InterfaceAlias      IPAddress       PrefixLength      Dhcp
  --------------      ---------       ------------      ----
  Ethernet            172.16.10.60    24                Enabled
  ```
- **Objectif d’export** (dans `export1.csv`) :
  ```
  Ethernet;172.16.10.60;24;Enabled
  ```
  
**Approche possible** :

1. Commence par récupérer la configuration de la carte réseau. Exemple de code :  
   ```powershell
   $ipConfig = Get-NetIPConfiguration
   ```
2. Récupère les informations souhaitées, par exemple :
   - `InterfaceAlias`  
   - `IPv4Address.IPAddress`  
   - `IPv4Address.PrefixLength`  
   - `IPv4Interface.Dhcp` (Enabled/Disabled)
3. Formate l’affichage. Tu peux utiliser `Select-Object` pour sélectionner et renommer des propriétés. Exemple :  
   ```powershell
   $ipConfig |
     Select-Object `
       @{Name='InterfaceAlias';Expression={$_.InterfaceAlias}}, `
       @{Name='IPAddress';Expression={$_.IPv4Address.IPAddress}}, `
       @{Name='PrefixLength';Expression={$_.IPv4Address.PrefixLength}}, `
       @{Name='Dhcp';Expression={$_.IPv4Interface.Dhcp}}
   ```
4. **Pour l’export CSV** avec `;` comme séparateur, tu peux faire :
   ```powershell
   $ipConfig | 
     Select-Object `
       @{Name='InterfaceAlias';Expression={$_.InterfaceAlias}}, `
       @{Name='IPAddress';Expression={$_.IPv4Address.IPAddress}}, `
       @{Name='PrefixLength';Expression={$_.IPv4Address.PrefixLength}}, `
       @{Name='Dhcp';Expression={$_.IPv4Interface.Dhcp}} |
     Export-Csv 'C:\Scripts\export1.csv' -NoTypeInformation -Force -Delimiter ';'
   ```
5. Assure-toi d’avoir un affichage en console **similaire** à celui requis.  
   - Tu peux par exemple utiliser `Format-Table` à la fin, mais en général un simple `Select-Object` dans la console s’affiche en tableau.

#### Capture(s) d’écran attendue(s) (Question 2)

1. **Le script `GetIpConfiguration_1.ps1`** (affichage de ton code).  
2. **La sortie dans la console** (tableau avec les 4 colonnes).  
3. **Le contenu de `export1.csv`** (montre-le via un éditeur de texte ou un `Get-Content`).

---

#### 2.2. Script `GetIpConfiguration_2.ps1`

- **Objectif d’affichage en console** :  
  ```
  Type d'interface : Ethernet
  Adresse IP :       172.16.10.60/24
  Statut du DHCP :   Enabled
  ```
- **Objectif d’export** (dans `export2.csv`) :  
  ```
  Ethernet;172.16.10.60/24;Enabled
  ```

**Approche possible** :

1. Toujours récupérer les informations réseau (similaire au script 1).  
2. Pour l’affichage, tu peux écrire simplement quelque chose comme :
   ```powershell
   Write-Host "Type d'interface : $($ipConfig.InterfaceAlias)"
   Write-Host "Adresse IP :       $($ipConfig.IPv4Address.IPAddress)/$($ipConfig.IPv4Address.PrefixLength)"
   Write-Host "Statut du DHCP :   $($ipConfig.IPv4Interface.Dhcp)"
   ```
3. Pour l’export, concatène l’IP et le préfixe comme dans l’affichage. Exemple :
   ```powershell
   $csvData = "$($ipConfig.InterfaceAlias);" +
              "$($ipConfig.IPv4Address.IPAddress)/$($ipConfig.IPv4Address.PrefixLength);" +
              "$($ipConfig.IPv4Interface.Dhcp)"

   $csvData | Out-File 'C:\Scripts\export2.csv'
   ```
   Ou utilise `">> export2.csv"` ou encore un `Export-Csv` personnalisé. L’important est qu’il n’y ait qu’une seule ligne et que le séparateur soit `;`.

#### Capture(s) d’écran attendue(s) (Question 3)

1. **Le script `GetIpConfiguration_2.ps1`** (affichage de ton code).  
2. **La sortie dans la console** (3 lignes comme dans l’exemple).  
3. **Le contenu de `export2.csv`** (une seule ligne avec 3 valeurs séparées par `;`).

---

## 2. Active Directory

### Contexte
- Machines concernées :  
  - **SRVWIN01** : Windows Server 2022 (AD-DS, DNS, DHCP)  
  - **CLIENT01** : Windows 10 (rejoint au domaine `lab.lan`)

### Intervention 1 : DHCP

> **Tâche**  
> Configurer le service DHCP pour que le client ait **dynamiquement** l’adresse IP `172.16.10.100`.

**Approche possible** :

1. Sur **SRVWIN01**, ouvre la console **DHCP** (`dhcpmgmt.msc`).  
2. Repère la plage d’adresses existante (Scope).  
3. Assure-toi que `172.16.10.100` est **à l’intérieur** de la plage ou crée une réservation si nécessaire.  
4. Si tu optes pour une réservation, fais un clic droit → « New Reservation » (Nouvelle réservation), saisis :  
   - **Reservation name** : par ex. `CLIENT01`  
   - **IP address** : `172.16.10.100`  
   - **MAC address** : récupère celle de **CLIENT01** via un `ipconfig /all` (ou en Hyper-V/VirtualBox)  
   - Type : « Both » (ou seulement DHCP si tu n’utilises pas BOOTP)  
5. Applique la configuration et, sur **CLIENT01**, fais un :  
   ```
   ipconfig /release
   ipconfig /renew
   ipconfig /all
   ```
   pour vérifier l’attribution en `172.16.10.100`.

#### Capture(s) d’écran attendue(s) (Question 4)

1. **Configuration DHCP** sur le serveur (montrant la réservation ou la plage d’adresses).  
2. **Sortie de `ipconfig /all`** sur le client, prouvant qu’il obtient `172.16.10.100`.

---

### Intervention 2 : GPO de fond d’écran par service

> **Tâche**  
> Créer une GPO `USER-Interface-Wallpaper-Gestion` pour l’OU « Gestion » (ou du moins pour les utilisateurs de ce service), qui :
> - Affiche `C:\Content\Gestion.jpg` en fond d’écran
> - A un filtrage appliqué uniquement aux utilisateurs du groupe AD correspondant (ou de l’OU)

**Approche possible** :

1. Ouvre **Group Policy Management** (`gpmc.msc`).  
2. Dans la structure de ton domaine `lab.lan`, repère l’OU `Gestion` (ou crée-la si besoin, en fonction du contexte).  
3. Crée une nouvelle GPO :  
   - Nom : `USER-Interface-Wallpaper-Gestion`  
   - Lien : sur l’OU `Gestion` (ou sur le domaine, puis filtrer le groupe de sécurité)  
4. Édite la GPO :  
   - **User Configuration** → **Policies** → **Administrative Templates** → **Desktop** → **Desktop** → **Desktop Wallpaper**  
   - Active la stratégie, et indique le chemin `C:\Content\Gestion.jpg`  
   - Sélectionne « Style » (remplir, étirer, centrer, etc.) selon le besoin  
5. **Filtrage de sécurité** :  
   - Retourne sur la vue « Scope » de la GPO  
   - Retire « Authenticated Users » si nécessaire  
   - Ajoute le groupe (ou l’OU) qui contient les utilisateurs de la gestion, avec les droits **Read + Apply Group Policy**  
6. Sur **CLIENT01**, connecte-toi avec un compte qui fait partie de « Gestion » et vérifie que le fond d’écran s’applique après un `gpupdate /force`.

#### Capture(s) d’écran attendue(s) (Question 5)

1. **Configuration de la GPO** :  
   - Au moins la partie où tu montres le paramètre « Desktop Wallpaper » pointant vers `C:\Content\Gestion.jpg`.  
   - La partie « Security Filtering » (filtrage), où on voit le groupe en question.  
2. **Le résultat sur le client** :  
   - Une copie d’écran montrant le fond d’écran (et/ou l’observation de la stratégie appliquée dans `gpresult /R`).

---

### Intervention 3 : Résolution DNS

> **Tâche**  
> Faire en sorte que le serveur IPBX (dont l’adresse IP est `172.16.10.5`) soit accessible par le nom **ipbx.lab.lan**.

**Approche possible** :

1. Sur **SRVWIN01**, ouvre la console **DNS** (`dnsmgmt.msc`).  
2. Dans la zone directe `lab.lan`, ajoute un **nouvel enregistrement hôte (A)** :  
   - **Name** : `ipbx`  
   - **IP address** : `172.16.10.5`  
3. Sur **CLIENT01**, teste :  
   ```
   ping ipbx.lab.lan
   ```
   ou un `nslookup ipbx.lab.lan`.

#### Capture(s) d’écran attendue(s) (Question 6)

1. **Configuration DNS** (l’enregistrement A créé).  
2. **Résultat du ping** (ou nslookup) sur le client.

---

## 3. Serveur Linux

### Contexte
- Machines concernées :  
  - **SRVLX01** (Debian 11) : GLPI, serveur web  
  - **CLIENT01** : Windows 10 (ou éventuellement SRVWIN01 pour se connecter en SSH)
- On veut sécuriser l’accès SSH sur SRVLX01 et mettre en place un site temporaire.

### Intervention 1 : Configuration SSH

> **Tâche**  
> - Interdire la connexion directe avec `root`  
> - Interdire la connexion par mot de passe (password)  
> - Autoriser la connexion par **clé SSH**  
> - Changer le port par défaut de 22 à **22504**  

**Approche possible** :

1. Sur **SRVLX01**, édite le fichier `/etc/ssh/sshd_config`.  
   - Cherche `PermitRootLogin yes` et mets `PermitRootLogin no`.  
   - Cherche `PasswordAuthentication yes` et mets `PasswordAuthentication no`.  
   - Ajoute ou modifie `Port 22504`.  
   - Assure-toi que `PubkeyAuthentication yes` est bien activé.  
2. Pense à **rediriger** le port dans VirtualBox (ou paramétrer le firewall local) si besoin, pour que depuis **CLIENT01** tu puisses joindre le port **22504**.  
3. Copie ta clé publique depuis **CLIENT01** (ou SRVWIN01) dans `~/.ssh/authorized_keys` (du compte Linux concerné, par ex. `wilder`).  
4. **Redémarre** le service SSH :  
   ```
   systemctl restart ssh
   ```
5. Sur **CLIENT01**, teste la connexion :  
   ```
   ssh -i C:\Users\wilder\.ssh\id_rsa wilder@172.16.10.15 -p 22504
   ```
   (ou avec n’importe quel outil SSH)  
6. Vérifie que la connexion se fait **sans mot de passe**, et que `root` n’est pas autorisé.

#### Capture(s) d’écran attendue(s) (Question 7)

1. **Configuration SSH** dans `/etc/ssh/sshd_config` (montrant les 4 points modifiés).  
2. **Preuve de la connexion SSH** depuis CLIENT01 (ou SRVWIN01), démontrant que tu es bien connecté sur `wilder@SRVLX01` via le port `22504`.

---

### Intervention 2 : Mise en place du site temporaire

> **Tâche**  
> - Dans le home directory de `root`, tu as 2 fichiers :
>   - `LisezMoi` (informations à lire)  
>   - `En-Travaux.tar.gz` (contient une image et un index pour un site temporaire)  
> - Ce site doit **remplacer** le site intranet actuel.  

**Approche possible** :

1. Connecte-toi en SSH ou localement.  
2. Regarde ce que contient `En-Travaux.tar.gz` :  
   ```
   tar -tvzf /root/En-Travaux.tar.gz
   ```
3. Extrais ces fichiers dans le répertoire correspondant à l’Intranet. Par exemple, si l’hôte virtuel intranet pointe sur `/var/www/html/intranet` (ou `/var/www/html` directement), alors :  
   ```
   cp /root/En-Travaux.tar.gz /var/www/html/
   cd /var/www/html
   tar -xvzf En-Travaux.tar.gz --overwrite
   ```
   (Assure-toi de supprimer ou renommer les anciens fichiers si besoin.)  
4. Vérifie les droits sur les fichiers (propriété, droits lecture).  
5. Depuis **CLIENT01**, navigue sur http://172.16.10.15 ou http://srvlx01 (selon la config DNS) et vérifie l’affichage du « site en travaux ».

#### Capture(s) d’écran attendue(s) (Question 8)

- **La page web affichée sur le client** prouvant qu’on voit bien le site temporaire.

---

## 4. Téléphonie

### Contexte
- Machines concernées :  
  - **SRVLX02** (Red Hat 7) : IPBX  
  - **SRVWIN01** : Windows Server 2022 (installera un softphone)  
  - **CLIENT01** : Windows 10 (installera un autre softphone)  
- On veut créer un nouveau compte SIP, puis tester un appel entre deux utilisateurs.

### Intervention 1 : Création de compte SIP sur l’IPBX

> **Tâche**  
> - Créer l’utilisateur **Miguel Hernandez** avec numéro **80104**.  
> - Montre la configuration côté IPBX et côté client.

**Approche possible** :

1. Connecte-toi à l’interface web de l’IPBX (Asterisk/FreePBX ou autre, selon le checkpoint).  
2. Ajoute une extension (ou un utilisateur SIP) :  
   - **Display name** : `Miguel Hernandez`  
   - **User/extension** : `80104`  
   - **Mot de passe SIP** : choisis/affiche-le pour le configurer dans le softphone.  
3. Applique la configuration (Submit + Apply config).  
4. Sur **CLIENT01**, lance un softphone (exemple : MicroSIP, Zoiper, ou autre).  
   - Configure un compte SIP :  
     - Serveur : `172.16.10.5` (IP de l’IPBX)  
     - Numéro ou Username : `80104`  
     - Mot de passe : celui défini sur l’IPBX  
5. Vérifie que ça s’enregistre correctement.

#### Capture(s) d’écran attendue(s) (Question 9)

1. **Configuration de l’utilisateur 80104** dans l’IPBX (interface web).  
2. **Preuve sur le client** que l’enregistrement est OK (softphone connecté ou capture de l’onglet « Accounts »).

---

### Intervention 2 : Test d’appel entre deux postes

> **Tâche**  
> - Mettre en service deux softphones :  
>   - L’un avec la ligne de **Miguel Hernandez** (80104)  
>   - L’autre avec la ligne de **Emma Chen** (un autre numéro, p.ex. 80103 ou 80102)  
> - Vérifier un appel réussi.

**Approche possible** :

1. Sur **SRVWIN01**, installe ou lance le même softphone. Configure le compte **Emma Chen**.  
2. Sur **CLIENT01**, on a le compte **Miguel Hernandez**.  
3. Fais un test d’appel :  
   - Depuis Miguel (80104) vers Emma (801xx)  
   - Sur l’interface, tu dois voir un appel entrant sur le softphone Emma.  
4. Décroche et prends une capture d’écran.

#### Capture(s) d’écran attendue(s) (Question 10)

- **Les deux softphones** (ou au moins un) montrant un appel **en cours** ou abouti.  

---

## Récapitulatif : Questions et Pièces à Rendre

Pour rappel, tu dois remplir le document **FichierReponseMiseEnSituationProfessionnelle** (protégé par un mot de passe) avec **uniquement** les **copies d’écran** demandées :

1. **Question 1** : Script `GetIpConfiguration.ps1` (modifications de commentaires).
2. **Question 2** : 
   - Script `GetIpConfiguration_1.ps1`  
   - Sortie console (tableau 4 colonnes)  
   - Contenu de `export1.csv`  
3. **Question 3** : 
   - Script `GetIpConfiguration_2.ps1`  
   - Sortie console (3 lignes)  
   - Contenu de `export2.csv`  
4. **Question 4** : Configuration DHCP (serveur) + `ipconfig/all` (client)  
5. **Question 5** : GPO fond d’écran (paramètres + filtrage) + résultat sur client  
6. **Question 6** : DNS (enregistrement `ipbx.lab.lan`) + `ping ipbx.lab.lan` sur client  
7. **Question 7** : Configuration SSH ( `/etc/ssh/sshd_config` ) + connexion SSH (port 22504)  
8. **Question 8** : Affichage du site temporaire sur le client (navigateur)  
9. **Question 9** : Création du compte SIP 80104 (IPBX) + preuve sur client (softphone)  
10. **Question 10** : Communication réussie entre Miguel et Emma (softphones)  

Après avoir compilé toutes ces captures, **convertis le document en PDF** et **uploade-le** en respectant les consignes de ton formateur.  

---

## Conseils Généraux

- **Sauvegarde et vérifie** tes configurations au fur et à mesure.  
- Utilise les **snapshots** sur VirtualBox si tu n’es pas sûr de tes manipulations.  
- N’hésite pas à documenter rapidement (même en local) chaque étape pour éviter les oublis.  
- Le temps est limité (1h30), donc organise tes priorités : certaines actions peuvent être en partie scriptées à l’avance, d’autres demandent des reboots, etc.

En suivant ce guide, tu auras une trame solide pour réussir chaque étape et produire les **preuves** requises. Bonne réussite pour ce checkpoint !
