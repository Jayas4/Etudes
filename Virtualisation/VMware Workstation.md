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
