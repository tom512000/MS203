# SAÉ 203 Modules Réseaux
Réalisée par **SIKORA Tom** et **DARROZES Guillaume** du groupe 3, et encadrée par **M. Arganini**.

## Objectifs :
- Installer et configurer un serveur Web complet
- Installer et configurer un serveur Web Apache
- Installer et configurer PHP
- Installer et configurer MySQL
- Installer et configurer phpMyAdmin
- Gérer des services sur un système Linux
- Obtenir une configuration locale similaire au serveur Web utilisé à l'IUT

## Notes
- Utilisation du préfixe `sudo`
- login : `iut`
- mot de passe : `iutinfo`
- Adresse IP : `10.31.5.249`

## 1. Gestion des services : `systemd`
**Travail à réaliser**
- `systemctl action cible [option(s)]` : Consultation du manuel de la commande systemctl.
- `systemctl` : Affichage de la liste des services démarrés avec la commande.
- `sudo apt-get install openssh-server` : Installation du paquet sshd.
- `ssh 10.31.5.249` : Vérification du démarrage du service sshd.
- `exit` : Déconnexion.
- `sudo systemctl stop ssh` : Arrêt du service sshd.
- `ssh 10.31.5.249` : Reconnexion.
- `sudo systemctl start ssh` : Redémarrage du service sshd.

## 2. Serveur Web apache2
**Travail à réaliser**
- `sudo apt-get install apache2` : Installation du paquet apache2 (4 erreurs).
- `sudo apt-get upgrade` : Mise à jour de tous les paquets installés sur le système.
- `sudo apt-get update` : Mise à jour des informations de dépôt de paquets sur le système.
- `sudo reboot` : Redémarrage du système d'exploitation.
- `sudo apt-get install apache2` : Réinstallation du paquet apache2.
- `systemctl start apache2` : Vérification du démarrage du service apache2.
- Vérification du bon fonctionnement du serveur Web (**http://10.31.5.249**).
- Exploration du contenu du répertoire */etc/apache2*.
- `cat apache2.conf` : Lecture du fonctionnement de la configuration au début du fichier.
- Les répertoires *conf-enabled*, *mods-enabled* et *sites-enabled* renvoient à des liens symboliques (répertoire qui pointe vers un autre emplacement dans le système de fichiers).
- `man a2enmod` : Consultation du manuel de la commande a2enmod.
- `sudo a2enmod userdir` : Activation des pages d'accueil des utilisateurs à l'aide du module userdir.
- `systemctl restart apache2` : Redémarrage du service apache2.
- `sudo mkdir public_html` : Création du répertoire *public_html* dans le répertoire d’accueil de l'utilisateur iut.
- `sudo chown iut:www-data /` : Attribution des droits d’accès à l’utilisateur du serveur Web www-data pour le répertoire d’accueil.
- `sudo chown iut:www-data public_html` : Attribution des droits d’accès à l’utilisateur du serveur Web www-data pour le répertoire *public_html*.
- `sudo chmod 755 public_html` : Affectation des droits par défaut pour le répertoire public_html.
- Vérification du bon fonctionnement du serveur Web (**http://10.31.5.249/~iut**).
- `sudo nano /etc/apache2/mods-available/userdir.conf` : Désactivation du mécanisme de listage des répertoires en supprimant l'option `Indexes` du fichier *userdir.conf*.
- Vérification de la désactivation du listages des répertoires (**http://10.31.5.249/~iut**).
- `touch public_html/bienvenue.html` : Création d'un fichier *bienvenue.html* dans le répertoire *public_html*.
- `sudo nano public_html/bienvenue.html` : Modification du fichier *bienvenue.html*.
- Vérification du bon fonctionnement du serveur Web (**http://10.31.5.249/~iut/bienvenue.html**).

**Configuration d'un serveur Web virtuel**
- `nslookup 10.31.5.249` : Récupération du nom DNS de notre machine virtuelle.
- Création d'un deuxième serveur Web (**http://2A4V1-31UVM249.ad-urca.univ-reims.fr**).
- `sudo mkdir ~/mon_serveur` : Création d'un répertoire de travail *mon_serveur* dans le répertoire d'accueil.
- `sudo chown iut:www-data mon_serveur/` : Attribution des droits d'accès au répertoire *mon_serveur* pour l'utilisateur www-data.
- `sudo ln -s /home/iut/mon_serveur /var/www` : Création d'un lien symbolique du répertoire *mon_serveur* vers l'arborescence */var/www*.
- `touch mon_serveur/index.html` : Création d'un fichier *index.html* dans le répertoire *mon_serveur*.
- `sudo nano mon_serveur/index.html` : Modification du fichier *index.html*.
- `cd /etc/apache2/sites-available` : Déplacement dans le répertoire */etc/apache2/sites-available*.
- `sudo cp 000-default.conf 2A4V1-31UVM249.conf` : Copie du fichier *000-default.conf* en *2A4V1-31UVM249.conf*.
- `sudo nano 2A4V1-31UVM249.conf` : Modification du fichier *2A4V1-31UVM249.conf*.
- `sudo a2ensite 2A4V1-31UVM249` : Activation du site.
- `systemctl reload apache2` : Rechargement de la configuration d'apache.
- Vérification de l'accès du serveur (**http://2A4V1-31UVM249.ad-urca.univ-reims.fr**).

## 3. Serveur Web sécurisé https
**Travail à réaliser**
- `dpkg -l | grep ssl` : Vérification de la présence des paquets libssl, openssl et ssl-cert.
- Aucun paquets manquants.
- `sudo mkdir /etc/apache2/ssl` : Création d'un répertoire pour stocker le certificat.
- `sudo /usr/sbin/make-ssl-cert /usr/share/ssl-cert/ssleay.cnf /etc/apache2/ssl/apache.pem` : Création du certificat correspondant à notre site dans le répertoire */etc/apache2/ssl*.
- `nano /etc/apache2/ports.conf` : Vérification du port d'écoute par défaut en ssl (443).
- `cp /etc/apache2/sites-available/default-ssl.conf /etc/apache2/sites-available/mon-serveur-ssl.conf` : Création du fichier de configuration VirtualHost de notre serveur ssl.
- `sudo nano /etc/apache2/sites-available/mon-serveur-ssl.conf` : Modification du fichier *mon-serveur-ssl.conf*.
- `a2enmod ssl` : Activation du module ssl d'apache.
- `a2ensite mon-serveur-ssl.conf` : Activation de notre site.
- `systemctl restart apache2` : Redémarrage du service apache2.
- Vérification de l'accès aux pages (**http://2A4V1-31UVM249.ad-urca.univ-reims.fr**).

## 4. Langage de programmation PHP
**Travail à réaliser**
- `sudo apt-get install php` : Installation du paquet php.
- `sudo apt-get install libapache2-mod-php` : Installation du paquet libapache2-mod-php.
- `sudo nano /etc/apache2/mods-enabled/php7.4.conf` : Modification du fichier *php7.4.conf*.
- `systemctl restart apache2` : Redémarrage du service apache2.
- `touch public_html/index.php` : Création d'un fichier *index.php* dans le répertoire *public_html*.
- `sudo nano public_html/index.php` : Modification du fichier *index.php*.
- Vérification de l'accès à la page (**http://10.31.5.249/~iut/index.php**).

## 5. Serveur de base de données MySQL
**Travail à réaliser**
- `sudo apt-get install mysql-server` : Installation du paquet mysql-server.
- `systemctl start mysql` : Démarrage du service mysql.
- `sudo mysql_secure_installation` : Configuration de la sécurité de mysql (mot de passe : iutinfo#S2).
- `mysql -u root -p` : Connexion en tant que root.
- `mysql> CREATE USER 'admin'@'localhost' IDENTIFIED BY 'admininfo#S2';` : Création de l'utilisateur admin avec le mot de passe admininfo#S2.
- `mysql> GRANT ALL PRIVILEGES ON *.* TO 'admin'@'localhost' WITH GRANT OPTION;` : Attribution des privilèges à l'utilisateur admin.
- `exit` : Sortie de mysql.
- `systemctl restart mysql` : Redémarrage du service mysql.
- `mysqlshow -u admin -p` : Teste du serveur.

## 6. Outil d'administration de bases de données phpMyAdmin
**Travail à réaliser**
- `sudo apt-get install phpmyadmin` : Installation du paquet phpmyadmin (problème de mot de passe).
- `mysql -u root -p` : Connexion en tant que root.
- `mysql> UNINSTALL COMPONENT 'file://component_validate_password';` : Suppression de la validation de mot de passe sécurisé.
- `sudo apt-get install phpmyadmin` : Réinstallation du paquet phpmyadmin (mot de passe : iutinfo).
- `sudo phpenmod mbstring` : Activation du module de gestion des chaînes de caractères multi-octets de php.
- `systemctl restart apache2` : Redémarrage du service apache2.
- Vérification de l'accès à la page (**http://2a4v1-31uvm249.ad-urca.univ-reims.fr/phpmyadmin/**).
- Création de l'utilisateur mysqltest (option "Créer une base portant son nom", tous les privilèges et mot de passe : sqltest#S2).
- Vérification de la connexion à phpMyMdmin avec l'utilisateur mysqltest.
