# DÉPLOIEMENT DE VOTRE SITE WEB  
## 1. Création de la machine virtuelle  

Nous allons créer une machine virtuelle sur Hyper-V. Assurez-vous d'avoir activé l'outil de virtualisation sur votre ordinateur.  

Ouvrez :  
`Panneau de Configuration` > `Programmes` > `Activer des fonctionnalités Windows` > `Hyper-V`  

Normalement, vous devrez redémarrer votre ordinateur afin d'avoir accès à Hyper-V.  
Une fois votre ordinateur redémarré, vous pouvez chercher dans les applications "*Hyper-V*".

Une fois Hyper-V lancé, voici les étapes à suivre :  

1. Rendez-vous sur Debian pour [télécharger l'image](https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/debian-12.9.0-amd64-netinst.iso) de la version 12.  
2. Rendez-vous sur Hyper-V pour "Créer un ordinateur virtuel" (menu à droite > Nouveau > Ordinateur Virtuel).  
3. Donnez un nom à la machine virtuelle (*Ex : website_local*). Vous pouvez également changer l'emplacement de votre machine virtuelle afin de la retrouver facilement à l'avenir.  
4. Sélectionnez la *`Génération 2`*.  
5. Laissez la mémoire de démarrage à *`1024 Mo`* et désactivez "Utiliser la mémoire dynamique", Linux n'a pas besoin de beaucoup de ressources pour fonctionner.  
6. Sélectionnez le choix de connexion, par défaut celui-ci sera "Non Connecté". De préférence, utilisez "Default Switch".  
7. Lors de la création du disque dur virtuel, vous devez "Créer un disque virtuel" et choisir comme emplacement celui de la machine virtuelle. Si une machine vous est fournie, vous pouvez choisir "Utiliser un disque existant".  
8. Passons maintenant à l'importation de l'image de Debian. Sélectionnez "*`Installer un système d'exploitation à partir d'un fichier image`*". Puis sélectionnez le fichier *.iso* que vous avez téléchargé au premier point.  
9. Un résumé de votre machine virtuelle s'affiche. Cliquez sur "*Terminer*" pour finaliser la création de la machine virtuelle.  

*Il est possible que Windows refuse de lancer la machine. Pour pallier ce problème, cliquez sur votre machine et rendez-vous dans les paramètres. Allez dans "Sécurité" et désactivez le démarrage sécurisé. Cliquez sur "Appliquer", vous pourrez désormais accéder à votre machine virtuelle.*

## 2. Configuration de la machine virtuelle  

Une fois lancée, un menu s'ouvre, nous choisirons "*Install*" (Installation depuis la console). Et c'est parti pour la configuration de votre machine virtuelle.  

*Votre souris sera inutile, vous utiliserez uniquement votre clavier. Utilisez les flèches pour vous déplacer, "Entrée" pour valider et "Espace" pour sélectionner.*  

1. Sélectionnez la langue (ici `French`).  
2. Sélectionnez la situation géographique (ici `France`).  
3. La sélection et la configuration du réseau automatique se lancent.  
4. Vous devez maintenant choisir le nom de votre machine (Ex : *Website*).  
5. Entrez un nom de domaine (Ex : *WebSite_Local.fr*).  
6. Une fois fait, la machine vous demande de choisir le mot de passe "Root". C'est le mot de passe du "Super Utilisateur", celui qui aura la totalité des droits sur votre machine.  
7. Après avoir confirmé le mot de passe, renseignez le nom de l'utilisateur par défaut (Ex : *Utilisateur*), ce sera votre compte principal.  
8. L'identifiant est le nom du compte qui sera utilisé, par défaut ce sera le nom de l'utilisateur sans majuscule.  
9. Choisissez un mot de passe pour votre compte. De préférence, différent de celui de Root.  
10. Une fois confirmé, les outils se téléchargent et se configurent automatiquement.  
11. Votre machine vous propose de partitionner votre disque, je vous conseille d'utiliser un disque entier.  
12. Après avoir cliqué, il vous propose de partitionner les dossiers. Nous considérerons que nous sommes débutants et choisirons "`Tout dans une seule partition`".  
13. Sélectionnez "`Terminer le partitionnement et appliquer les changements`". Puis sélectionnez "`Oui`" pour appliquer les changements apportés.  
14. Le système s'installe...  
15. Lorsque votre machine virtuelle vous demande de continuer sans miroir, sélectionnez "`Oui`", nous l'installerons plus tard.  
16. Si votre machine vous demande de participer aux études statistiques, sélectionnez "`Non`".  
17. Normalement, les logiciels nécessaires s'installent et votre machine termine son installation. Il est possible qu'elle vous demande de "redémarrer". Sélectionnez "`Continuer`" puis ne touchez plus à rien.  
18. Une fois que votre machine vous demande votre "login", entrez votre nom d'utilisateur saisi au point huit, puis votre mot de passe.  

Félicitations, vous venez de finir de configurer votre machine virtuelle !
## 3. Configuration APT
APT est un gestionnaire de paquets utilisé sur les systèmes Debian et ses dérivés. Il permet d'installer, mettre à jour et supprimer des logiciels de manière simple en gérant les dépendances automatiquement.

1. Une fois connecter, fait la commande `SU` et renseigne le mot de passe du compte root que vous avez choisit dans [le point 6](#2-configuration-de-la-machine-virtuelle).
2. Une foit connecter avec le compte root, le symbole **$** derière votre nom d'utilisateur est remplacé par **#**.
3. Executer la commande `nano /etc/apt/sources.list`. Cela ouvre un editeur de texte intégré à la console. Si celui-ci est vide, inserez le texte suivant :  
```
deb http://deb.debian.org/debian/ bookworm main non-free-firmware
deb-src http://deb.debian.org/debian/ bookworm main non-free-firmware

deb http://security.debian.org/debian-security bookworm-security main non-free-firmware
deb-src http://security.debian.org/debian-security bookworm-security main non-free-firmware

deb http://deb.debian.org/debian/ bookworm-updates main non-free-firmware
deb-src http://deb.debian.org/debian/ bookworm-updates main non-free-firmware
```
Pour enregistrer faite `Ctrl + X`. Cela devrait installer les fichiers nécéssaire.

## 4. Installation du Serveur SSH
Avant d'installer et de le configurer, répondons à la question :  
**Qu'es ce que SSH ?**  
Secure Shell est un protocole réseau permettant une connexion sécurisée à un système distant. Il est principalement utilisé pour l'administration de serveurs, le transfert de fichiers et l'exécution de commandes à distance de manière chiffrée.

1. Afin d'installer les services SSH, effectuer cette commande :  
`apt update && apt install -y openssh-server`
2. Redémare le service afin de l'activé avec :  
`systemctl start ssh`
3. Vous pouvez vérifier que le service soit installer avec la commande :  
`systemctl status ssh`
4. Si tout est installer correctement, il devrait y avoir ses lignes :
```
ssh.service - OpenBSD Secure Shell server
    Loaded: loaded (/lib/systemmd/system/ssh.service; enabled; preset: enabled)
    Active: active (running) since ...
```
## 5. Installation de Putty
**Qu'es ce que Putty ?**
C'est un émulateur de terminal doublé d'un client pour les protocoles SSH et TCP brut. Il permet d'effectuer des connexions directes. À l'origine disponible uniquement pour Windows, il est à présent porté sur diverses plates-formes Unix.

Pour installer Putty, vous pouvez cliquez sur le lien [**ci-après**](https://the.earth.li/~sgtatham/putty/latest/w64/putty-64bit-0.83-installer.msi).

1. Dans la console de la machine virtuelle, effectuez la commande `ip addres`.
2. Ouvrez Putty
3. Dans le champs "Host Name", mettez l'addresse IP de la VM. (Machine Virtuelle).
4. Mettez le port "22" et choisissez la connexion "SSH".
5. Cliquez sur "Open". Une consolle est sencés'ouvrir.
6. Si c'est votre première connexion depuis la VM une fenetre est sencé s'ouvrir pour vous demander votre autorisation de connexion, cliquez sur "Accept" pour valider la connexion.
7. `login as :` est sencé s'afficher. Entrez votre nom d'utilisateur. Puis entrez votre mot de passe (si rien ne s'affiche quand vous tapez, c’est normal, linux protège les mots de passes en les masquants.)
8. Si tout s'est bien passé, vous devrez voir une ligne comme : `debian@[NOM DE LA VM]:~$`. Cela signifie que vous vous êtes connecté en SSH à la VM.
9. Pour vérifier que tout est opérationnel, effectuez la commande `whoami` cela est sencé afficher "*root*".
10. Effectuez la commande `hostname -I`, ce qui est sencé vous donner l'adresse IP de votre machine virtuelle.
## 6. Configuration FTP
1. Pour installer le serveur FTP executer la commande :  
`apt install vsftpd`
2. Vous pouvez vérifier que le serveur a été correctement installer avec :  
`systemctl status vsftpd`
3. Des lignes similaire à [SSH](#4-installation-du-serveur-ssh) devrait s'afficher. Si ce n'est pas le cas, veuillez le start :  
`systemctl start vsftpd`
4. Vous pouvez modifier le fichier de configuration de "vsftpd" en faisant la commande :  
`nano /etc/vsftpd.conf`
5. Modifiez le fichier en effectuant ses modification ou ajoutez les s'il elles n'existent pas :
> `anonymous_enable=NO`  
> `local_enable=YES`  
> `write_enable=YES`
6. Pour enregistrer faite `Ctrl + O` puis `Ctrl + X` pour quitter.
7. Redémarrer le serveur FTP avec `systemctl restart vsftpd`.
## 7. Testez avec FileZilla
1. S'il n'est pas installer sur votre poste principal (pas la VM), [**installez le ici**](https://download.filezilla-project.org/client/FileZilla_3.68.1_win64_sponsored2-setup.exe).
2. Ouvrez le et renseignez les informations suivantes :
> **Hôte** : IP de la machine virtuelle  
> **Identifiant** : Nom d'utilisateur de la VM  
> **Mot de passe** : Mot de passe du compte utilisateur de la VM  
> **Port** : 21  
3. Cliquez sur "Connexion rapide".
4. Vous devriez voir les fichiers de votre dossier personnel à droite.
## 8. Serveur Web LAMP
LAMP est l'accronyme de `L`inux `A`pache `M`ySQL `P`HP. Cela désigne un ensemble de logiciels libres permettant de construire des serveurs de sites web.
1. Sur votre machine virtuelle, installez Apache, PHP et MySQL. Avec la commande :  
`apt update`  
`apt install apache2 php libapache2-mod-php mariadb-server php-mysql`
2. Vous pouvez vérifier que tous les services sont actifs :  
`systemctl status apache2`  
`systemctl status mariadb`
3. Le status "*active (running)*" devrais être visible pour les deux.
4. Afin de vérifier que tout c'est passé correctement, aller sur la machine principal et allez sur votre navigateur.
5. Dans la barre de recherche, tappez l'addresse IP de la VM : `http://[IP DE LA VM]`.
6. Si tout va bien, vous devrez voir la page "*Apache2 Debian Default Page*".
7. Dans la machine virtuelle, effectuez la commande :  
`echo "<?php phpinfo(); ?>" | tee /var/www/html/acceuil.php`
8. Dans le navigateur de la machine principal, ajoutez "acceuil.php" à la fin de la recherche :  
`http://[IP DE LA VM]/acceuil.php`
9. Si un tableau avec plein d'info concernant PHP s'affiche cela signifie que tout fonctionne parfaitement.
10. Vous pouvez supprimer le fichier en faisant la commande :  
`rm /var/www/html/acceuil.php`
## 9. Création d'un VHOST et un domaine local
Désormais nous allons créer un nom de domaine pour eviter de tapper l'IP dans le navigateur.
1. Dans la machine virtuelle, effectue la commande :  
`mkdir -p /var/www/[NOM DOMAINE].local`  
__Exemple__: *mkdir -p /var/www/john.local*
2. Créez une page d'acceuil (ici *index.html*), utilisez la commande :  
`echo "<h1>Bienvenue sur mon site</h1>" | tee /var/www/[NOM DOMAINE].local/index.html`
3. Attribuez les droits à Apache au dossier "*[NOM DOMAINE].local*".  
Utilisez les commandes suivantes :  
`chown -R www-data:www-data /var/www/[NOM DOMAINE].local`  
`chmod -R 755 /var/www/[NOM DOMAINE].local`
> **755 donne les permissions** :  
> 7 = lecture + écriture + exécution  
> 5 = lecture + exécution
> 
> **1er chiffre** = droits du propriétaire  
> **2e chiffre** = droits du groupe  
> **3e chiffre** = droits des autres utilisateurs
4. Créez le fichier VHOST avec la commande :  
`nano /etc/apache2/sites-available/[NOM DOMAINE].local.conf`  
Inserez le contenu suivant en remplassant "*[NOM DOMAINE]*" par votre nom de domaine :
```
<VirtualHost *:80>
    ServerName [NOM DOMAINE].local
    ServerAlias www.[NOM DOMAINE].local
    DocumentRoot /var/www/[NOM DOMAINE].local

    <Directory /var/www/[NOM DOMAINE].local>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/[NOM DOMAINE]_error.log
    CustomLog ${APACHE_LOG_DIR}/[NOM DOMAINE]_access.log combined
</VirtualHost>
```
5. Activez le site en effectuant les commandes :  
`a2ensite [NOM DOMAINE].local.conf`  
`systemctl reload apache2`
6. Modifiez le fichier "*hosts*" sur Windows (machine principal).  
Rendez vous dans le fichier `C:\Windows\System32\drivers\etc\hosts` de votre machine principale.  
Ajoutez ça à la fin :
```
[IP VM]     [NOM DOMAINE].local
[IP VM]     www.[NOM DOMAINE].local
```  
Il est possible que vous ne possediez pas les accès nécéssaires. Dans ce cas suivez les étapes suivantes :  
> - Cliquez sur *Démarrer*  
> - Tapez *Bloc-notes*
> - Ouvrez "Bloc-notes" en tant qu’administrateur
> - Dans le Bloc-notes ouvert allez dans **Fichier > Ouvrir**
> - Allez dans `C:\Windows\System32\drivers\etc` et séléctionnez "Tous les fichiers"
> - Cliquez sur `hosts` puis sur **Ouvrir**
> - Une fois édité enregistrez le fichier et fermez le.
7. Allez dans le navigateur de votre machine principale, vous pouvez inscrire dans la barre de recherche : "*http://[NOM DOMAINE].local*"

Normalement ce que vous avez fait dans l'[étape 2](#9-création-dun-vhost-et-un-domaine-local) devrais s'afficher.
## 10. Importer le site depuis Github
1. Avant d'importer le [projet Github](https://github.com/Augustin-Erceville/Cinema), nous allons devoir installer git sur la VM.  
Il est fort possible que vous le site ne soit pas fonctionnel dû au fait que vous n'ayez pas installer la [base de donnée](https://github.com/Augustin-Erceville/Cinema/blob/f0890d0e081fd10698e14cf9ea282c8ceef37de3/documentations/cinema.sql).
2. Pour installer git, effectuez les commandes suivantes :  
`sudo apt update`  
`sudo apt install git -y`
3. Faite un nettoyage des fichiers actuel du site avec la commande :  
`sudo rm -r /var/www/[NOM DOMAINE].local/*`
4. Importez le repository Github afin d'importer les fichiers de votre projet.  
Ici, nous utiliseront le repository de mon projet "[Cinema](https://github.com/Augustin-Erceville/Cinema.git)" réalisé en cours de BTS.  
Utilisez la commande ci-dessous :  
`git clone https://github.com/Augustin-Erceville/Cinema.git /var/www/[NOM DOMAINE].local`
5. Après avoir cloner le repository, assurez vous de donner les bons droits à Apache :  
`chown -R www-data:www-data /var/www/[NOM DOMAINE].local`  
`chmod -R 755 /var/www/[NOM DOMAINE].local`
6. Vous pouvez maintenant tester le tout dans votre navigateur de votre machine principale :  
`http://[NOM DOMAINE].local`
7. Si tout est bien configurer, vous devriez voir le site. N'oubliez pas d'installer la BDD afin de voir correctement les pages.
---
