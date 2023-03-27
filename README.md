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
- `systemctl action cible [option(s)]` : Consultation du manuel de la commande systemctl.
- `systemctl` : Affichage de la liste des services démarrés avec la commande.
- `sudo apt-get install openssh-server` : Installation du paquet sshd.
- `ssh 10.31.5.249` : Vérification du démarrage du service sshd.
- `exit` : Déconnection.
- `sudo systemctl stop ssh` : Arrêt du service sshd.
- `ssh 10.31.5.249` : Reconnection.
- `sudo systemctl start ssh` : Redémarrage du service sshd.
