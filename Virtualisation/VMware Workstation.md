Tutoriel : Installation et configuration d’un environnement virtuel
sous Debian
user root : root
user jt : root
Tout d’abord 🤓 , vous devezlocaliser 🔎 l’application VMWare , rendez vous sur votre bureau 🖥️ et cliquez sur cette icone : 


Maintenez sur votre clavier la touche CTRL et N
Cela permet de créer ✏️ une machine virtuelle sans OS 


Ensuite cliquez le petit rond , à la gauche de typical (Recommandé)puis cliquez sur Next









Cliquez ensuite à gauche , sur le petit rond à côté de : I will install the operating system later puis cliquez sur next :




Cliquez ensuite à côté de linux sur le petit rond, puis dans le menu déroulant , cliquez dessus puis choisissez  : Debian 11 .x64-bit puis cliquez sur Next


Cliquez ensuite sur next 

Puis cliquez sur sur finish

Vous avez alors accès aux périphériques qui composent la machine virtuelle

Sélectionner la carte son, puis supprimer-la. Faites de même pour l’imprimante si elle est présente.







10.Dans « CD/DVD », utiliser l’image ISO de Debian qui est présente sur le bureau de l’ordinateur.
sélectionner browse puis selectionner le fichier .iso

11.Pour que votre serveur soit accessible sur le réseau, il va falloir configurer sa carte réseau.
Sélectionner-là, puis modifier ses propriétés.











C. Installation du système
1. Pour démarrer la machine, cliquer simplement sur « power on virtual machine ».

Appuyez ensuite sur entré




Cliquer sur France puis continuer





Cliquez encore une fois sur francais puis continuer


Choisisez un nom de machine , par defaut debian donc cliquer sur continuez 






Ignorer cette étape en cliquant sur continuer

Choisisez maintenant un mot de passe pour root , vous pouvez mettre root , et pour afficher le mot de passe cliquer a gauche de la petite case afficher le mot de passe en clair , puis répétez le encore une fois en bas  , puis cliquez sur continuer








Crée un nouvelle utilisateur , entrez son nom , puis cliquez sur continuer

Entrez encore une fois un mot de passe  vous pouvez mettre root , et pour afficher le mot de passe cliquez à gauche de la petite case afficher le mot de passe en clair , puis répétez le encore une fois en bas  , puis cliquez sur continuer









Cliquez sur Assisté - Utiliser un disque entier , puis cliquez sur continuer

Choisiser votre disque , avec marqué VMware dessus puis cliquez sur continuer








Cliquez ensuite sur tout dans une seule partition , puis cliquez sur continuer

Puis cliquez sur Terminez le partitionnement et appliquez les changement puis cliquez ure continuer







Cliquez ensuite sur Oui , puis continuer




10. Le système se configure puis se termine en vous demandant de redémarrer. Décocher le lecteur
de cd virtuel dans les settings de votre VM, et cliquer sur « continuer ».
11. Le système est maintenant fonctionnel, et boot sur l’invite de commande, avec la demande de
login.



D)Administration du serveur

Nous sommes maintenant devant un système d’exploitation vide, sans interface graphique qui plus
est ! Nous allons devoir effectuer quelques réglages pour pouvoir administrer le système plus
facilement.
1. Commencer par vous logguer sur la machine : saisir l’identifiant de votre utilisateur, puis son
mot de passe. Attention, par défaut, le pavé numérique n’est pas actif, et comme à chaque fois
sous Linux, la saisie du mot de passe n’entraine aucun retour graphique.
2. La première opération consiste à mettre à jour le système, pour cela, utiliser la commande
suivante : apt update && apt upgrade -y
La première commande va chercher la liste des paquets disponibles, et la seconde va procéder
aux mises à jour. Malheureusement, cela ne fonctionne pas, pour une question de droits. En
effet, les paquets faisant partie des fichiers système, ils ne sont pas accessibles à un utilisateur
standard.


3. Pour exécuter certaines commandes, il est nécessaire d’utiliser les droits de l’utilisateur « root »,
mais c’est assez risqué, pour des raisons de sécurité. Il existe donc une commande pour exécuter
des instructions en tant que « root », il s’agit de la commande « sudo ». Celle-ci n’est pas installé
par défaut sur Debian, nous allons devoir la rajouter. Pour cela, il faut :
a. Changer d’utilisateur en sortant avec la commande exit.
b. A la nouvelle demande de login, se connecter en tant que root.
c. Faire les mises à jour du système avec la commande vue en D.2.
d. Installer sudo avec apt-get install sudo
e. Ajouter votre utilisateur au groupe sudo pour pouvoir utiliser des commandes
système avec adduser eleve sudo si votre utilisateur s’appelle eleve.
Vous pouvez maintenant sortir avec exit pour revenir au login et vous authentifier avec votre
compte utilisateur, puis tester les manipulations précédentes en tentant une mise à jour avec la
commande suivante : sudo apt update && sudo apt upgrade -y. Saisir le mot de passe du compte
utilisateur, et normalement, la procédure de mise à jour va se dérouler normalement.

4. Il n’est pas très pratique de saisir des instructions via l’invite de commande depuis l’interface de
WorkStation, nous allons donc installer un serveur ssh sur notre Debian pour pouvoir nous
connecter directement depuis une autre machine, et donc pouvoir procéder à des copier/coller.
Pour cela, nous allons installer openssh-server avec les instructions suivantes :
a. sudo apt install openssh-server
b. Valider l’installation avec “o” quand le système demandera votre accord.
c. Pour faciliter la connexion, on doit permettre le login des utilisateurs du groupe
superutilisateur, et puisqu’il s’agit d’un environnement de développement et non pas
d’une solution de production, nous allons nous permettre quelques latitudes avec la
sécurité. Éditer le fichier de configuration avec la commande
sudo nano /etc/ssh/sshd_config
d. Il faut utiliser les flèches de navigation pour aller jusqu’à la section « Authentication : »,
puis ajouter une ligne en dessous de « #PermitRootLogin prohibit-password ». On
pourrait réactiver cette ligne en enlevant le « # », mais cela ne permettrait de se logguer
qu’avec des clés de sécurité qu’il sera trop long de mettre en œuvre, on va donc tout
simplement ajouter la ligne PermitRootLogin yes.



e. Pour prendre en compte cette modification, il faut enregistrer avec « Ctrl + O », puis
quitter avec « Ctrl + X ».
f. Il faut maintenant redémarrer le serveur openssh avec la commande sudo systemctl
restart --now ssh.service (attention au double tiret !).
5. On va maintenant tester la connexion depuis une autre machine. On doit d’abord récupérer
l’adresse IP de la VM avec la commande ip address (ou ip a pour les feignants...).

Afin de limiter l’utilisation du DHCP sur le réseau, on va utiliser une @ IP fixe pour notre VM.
Pour cela, on va utiliser les commandes suivantes :
a. sudo nano /etc/network/interfaces. On repère la section “primary network interface”,
et on remplace “dhcp” par “static ».
b. On définit ensuite les paramètres IP comme sur la capture d’écran ci-dessous, puis on
enregistre les modifications avec Ctrl + O, puis on quitte avec Ctrl + X.

c. On redémarre l’interface réseau avec les commandes sudo systemctl restart
networking.service et sudo ifup ens33, où ens33 désigne le nom de notre carte réseau
(voir capture d’écran ci-dessus). Si vous vérifiez avec la commande ip a, vous devriez
constater que votre IP est bien 192.168.3.34 maintenant.
d. Vous pouvez maintenant tester la connexion directement depuis Windows : Faire Win +
R puis saisir CMD pour accéder à l’invite de commande de Windows, et entrer la
commande suivante : ssh root@192.168.3.X.

E. Installation et configuration de LAMP
Nous sommes maintenant connectés en ssh directement depuis Windows, nous pouvons donc
nous déconnecter de l’invite de commande de notre VM avec la commande exit.
1. La première installation à faire est celle d’un interpréteur html, comme Apache2, qui va
permettre la génération des pages web à destination de notre navigateur. Pour cela, on utilise
la commande apt install apache2. Le fait d’être connecté en tant que root nous dispense
d’avoir à ajouter « sudo » devant chaque commande. Il est maintenant possible de sélectionner
les commandes ici, de les copier, puis de les transmettre à notre client ssh en faisant un clic
droit, ce qui va automatiquement coller le contenu du presse-papier. Valider l’installation avec
« O ». On démarre le service Apache2 avec la commande systemctl enable apache2.service,
ce qui permettra à Apache de démarrer en même temps que votre Debian.
pour voir si apache est bien en marche : 


2. Pour vérifier le bon déroulement de l’installation, il suffit de lancer un navigateur web, et
d’accéder à la page 192.168.3.X, vous devriez voir apparaitre le fameux « It works ! » d’Apache.

3. La deuxième installation concerne l’interpréteur php, permettant de générer des pages
dynamiques. Pour cela, on utilise la commande apt install php -y. Pour vérifier la version
installée, il est nécessaire de saisir un code particulier à la racine de notre hébergement, en
éditant un fichier avec la commande nano /var/www/html/index.php, et en y ajoutant le code
suivant :

<?php
Phpinfo();
?>

Pensez bien à enregistrer et quitter le fichier avec Ctrl + O et Ctrl + X.
On récupère ensuite les informations de version en accédant depuis un navigateur web à
l’adresse 192.168.3.X/index.php.


Quelle est donc le numéro de la version de PHP qui est présente sur votre Debian ?
La version de PHP de notre debian est : PHP 7.4.33
4. La troisième et dernière installation nécessaire est celle de la base de données, qui servira à
stocker les résultats des requêtes php et à en générer de nouvelles. Il existe plusieurs solutions
de bases de données, comme PostGre, MySQL, ou MariaDb. Nous allons installer cette dernière
solution avec la commande apt install mariadb-server -y, puis on automatise son démarrage
avec la commande systemctl enable mariadb.service.

5. Administration de la base de données : L’administration basique se fait en ligne de commande,
on va donc commencer par se connecter avec mysql -u root.

a. Création d’une base de données : CREATE DATABASE exemple ;
b. Création d’un user et du mdp : CREATE USER eleve@localhost IDENTIFIED BY ‘26400’;
c. Attribution des droits : GRANT ALL PRIVILEGES ON exemple.* TO eleve@localhost;
d. Rafraichissement des droits : FLUSH PRIVILEGES ;


Nous avons ensuite fini ! 🎉

                TIMEO ET JAYA
