# 📚 Tutoriel : Installation et configuration d’un environnement virtuel sous Debian

**Utilisateurs :**
- `root`
- `jt` (également root)

---

## A. Création de la machine virtuelle avec VMware

1. Localisez l’application **VMware** sur le bureau 🖥️ et cliquez sur l’icône.

2. Appuyez sur `CTRL + N` pour créer une nouvelle machine virtuelle **sans OS**.

3. Sélectionnez **Custom (avancé)** puis cliquez sur `Next`.

4. Cochez **"I will install the operating system later"**, puis cliquez sur `Next`.

5. Sélectionnez :
   - **Type** : Linux
   - **Version** : Debian 11.x 64-bit  
   Puis cliquez sur `Next`.

6. Continuez avec les options proposées et cliquez sur `Finish`.

7. La machine virtuelle est maintenant créée. Supprimez les périphériques inutiles :
   - Supprimez la **carte son**
   - Supprimez l’**imprimante** si elle est présente

8. Dans l’onglet **CD/DVD**, utilisez l’image ISO de Debian :
   - Cliquez sur **Browse**
   - Sélectionnez le fichier `.iso` situé sur le bureau

9. Pour que la machine soit accessible sur le réseau :
   - Sélectionnez la carte réseau
   - Modifiez ses propriétés pour qu’elle soit en **bridge** (mode pont)

---

## B. Installation du système Debian

1. Démarrez la VM avec **Power on virtual machine**.

2. Appuyez sur `Entrée` pour démarrer l’installation.

3. Sélectionnez **France**, puis cliquez sur `Continuer`.

4. Sélectionnez **Français** comme langue, puis `Continuer`.

5. Entrez un nom de machine (ex: `debian`), puis cliquez sur `Continuer`.

6. Ignorez la configuration du domaine en cliquant sur `Continuer`.

7. Définissez le mot de passe pour `root` (ex: `root`) :
   - Cochez la case pour afficher le mot de passe
   - Répétez le mot de passe et cliquez sur `Continuer`

8. Créez un nouvel utilisateur :
   - Entrez son prénom/nom (ex: `eleve`), puis cliquez sur `Continuer`
   - Définissez un mot de passe utilisateur (ex: `root`), cochez pour l’afficher, répétez, puis cliquez sur `Continuer`

9. Choisissez :
   - **Assisté - utiliser un disque entier**
   - Sélectionnez le disque `VMware`, puis `Continuer`
   - Choisissez **Tout dans une seule partition**, puis `Continuer`
   - Cliquez sur **Terminer le partitionnement et appliquer les changements**, puis `Continuer`
   - Confirmez avec **Oui**, puis `Continuer`

10. Une fois l'installation terminée :
    - Décochez le lecteur CD dans les paramètres de la VM
    - Cliquez sur `Continuer` pour redémarrer

11. Debian démarre et affiche l’invite de connexion (`login:`)

---

## C. Administration de base

1. Connectez-vous avec l’utilisateur créé :
   - Tapez l’identifiant, puis le mot de passe
   - ⚠️ Le pavé numérique n’est pas activé par défaut, et le mot de passe ne s’affiche pas

2. Mettez à jour le système :
   ```bash
   apt update && apt upgrade -y


> Cette commande échouera sans les droits administrateur :

```bash
apt update && apt upgrade -y
```

### 🧑‍💼 Devenir administrateur (root)

1. Tapez `exit` pour revenir à l’écran de connexion.
2. Connectez-vous en tant que **root**.
3. Refaites la mise à jour :
```bash
apt update && apt upgrade -y
```
4. Installez sudo :
```bash
apt install sudo
```
5. Ajoutez votre utilisateur au groupe `sudo` (ex: `eleve`) :
```bash
adduser eleve sudo
```
6. Tapez `exit`, reconnectez-vous avec votre utilisateur et testez :
```bash
sudo apt update && sudo apt upgrade -y
```

---

## 🖧 Accès SSH à distance

### 🔌 Installez le serveur SSH :
```bash
sudo apt install openssh-server
```

### 🔐 Activez la connexion root

1. Éditez le fichier de configuration :
```bash
sudo nano /etc/ssh/sshd_config
```
2. Sous la section `Authentication`, ajoutez :
```nginx
PermitRootLogin yes
```
3. Enregistrez avec **Ctrl + O**, quittez avec **Ctrl + X**.

4. Redémarrez le service SSH :
```bash
sudo systemctl restart --now ssh.service
```

### 🌐 Récupérez l’adresse IP :
```bash
ip a
```

### 📡 Passez l’adresse en IP fixe :

1. Éditez le fichier :
```bash
sudo nano /etc/network/interfaces
```
2. Remplacez `dhcp` par :
```nginx
iface ens33 inet static
address 192.168.3.34
netmask 255.255.255.0
gateway 192.168.3.1
dns-nameservers 8.8.8.8
```
3. Redémarrez le réseau :
```bash
sudo systemctl restart networking.service
sudo ifup ens33
```

### 🧪 Testez depuis un autre PC :
1. Ouvrez CMD (Win + R > `cmd`)
2. Tapez :
```bash
ssh root@192.168.3.34
```

---

## 🔧 Installation du serveur LAMP

### 🌍 Installez Apache2 :
```bash
apt install apache2
systemctl enable apache2.service
```

### ✅ Vérifiez dans un navigateur :
Accédez à [http://192.168.3.34](http://192.168.3.34) — vous devriez voir **It works!**

### 🐘 Installez PHP :
```bash
apt install php -y
```

### 🧪 Vérifiez la version de PHP :
1. Créez un fichier :
```bash
nano /var/www/html/index.php
```
2. Ajoutez :
```php
<?php
phpinfo();
?>
```
3. Enregistrez avec **Ctrl + O**, quittez avec **Ctrl + X**.

4. Accédez à [http://192.168.3.34/index.php](http://192.168.3.34/index.php) pour voir la version PHP.

📌 Version attendue : `PHP 7.4.33`

### 🗃️ Installez MariaDB :
```bash
apt install mariadb-server -y
systemctl enable mariadb.service
```

### ⚙️ Administration de la base de données :
```bash
mysql -u root
```

Dans MySQL :
```sql
CREATE DATABASE exemple;
CREATE USER 'eleve'@'localhost' IDENTIFIED BY '26400';
GRANT ALL PRIVILEGES ON exemple.* TO 'eleve'@'localhost';
FLUSH PRIVILEGES;
```

---

🎉 **Félicitations !**  
Vous avez maintenant un serveur Debian virtuel fonctionnel avec LAMP installé et accessible à distance !

> Par *Timeo & Jaya*
