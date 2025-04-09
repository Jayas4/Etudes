
# 🐧 Installation et Configuration d’un Serveur Debian avec LAMP

## 🔐 Activation des droits administrateur

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
