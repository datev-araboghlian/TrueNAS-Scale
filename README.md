# TrueNAS-Scale

Documentation du Projet TrueNAS Scale

1. Introduction

TrueNAS Scale est un système d'exploitation NAS (Network Attached Storage) open-source basé sur Debian Linux. Il prend en charge la virtualisation, la conteneurisation (Docker/Kubernetes) et divers protocoles de partage de fichiers tels que CIFS/Samba, NFS et iSCSI. Ce projet implique l'installation de TrueNAS Scale sur une machine virtuelle, la configuration d'un stockage en RAID 6 et la mise en place des connexions SSH, HTTPS et SFTP.

2. Installation de TrueNAS Scale

Démarrer la VM TrueNAS Scale avec l’ISO monté.

Suivre les étapes d'installation :

Sélectionner le disque d’installation (16 Go)

Configurer le mot de passe administrateur

Finaliser l’installation et redémarrer

Accéder à l’interface web via l’adresse IP attribuée :

ip addr show

3. Configuration du Stockage RAID 6

Accéder à Storage > Pools et créer un nouveau pool.

Ajouter les 5 disques de 2 To et configurer en RAID 6.

Nommer le pool Stockage et valider.

4. Connexion à TrueNAS Scale

HTTPS: Ouvrir un navigateur et entrer https://<TrueNAS_IP>.

SSH: Activer SSH dans System Settings > Services puis se connecter :

ssh admin@<TrueNAS_IP>

SFTP: Activer et configurer SFTP dans Services.

5. Activation des Services

Aller dans System Settings > Services.

Activer et configurer SSH et FTP.

Modifier le port SFTP (ex: 2222) :

echo "Port 2222" >> /etc/ssh/sshd_config
systemctl restart ssh

6. Configuration des Utilisateurs et Sessions SFTP

Créer plusieurs utilisateurs avec leurs répertoires dédiés :

useradd -m -s /bin/bash user1
passwd user1
mkdir /mnt/Stockage/user1
chown user1:user1 /mnt/Stockage/user1

Définir un dossier public :

mkdir /mnt/Stockage/Public
chmod 777 /mnt/Stockage/Public

Tester les connexions SFTP avec un client comme FileZilla.

7. Installation de Docker et Vaultwarden

Activer Docker :

systemctl enable docker
systemctl start docker

Installer Vaultwarden :

docker run -d --name vaultwarden \
  -v /mnt/Stockage/vaultwarden:/data \
  -p 8080:80 \
  --restart unless-stopped \
  vaultwarden/server:latest

Vérifier que le service est opérationnel :

docker ps

8. Création d'une VM Debian sous TrueNAS Scale

Aller dans Virtual Machines > Add.

Configurer avec les paramètres :

Utilisateur: LaPlateforme

Mot de passe: LaPlateforme13

Tester la connexion SSH à cette VM.

9. Tests et Validation

Vérifier l’accès réseau (HTTPS, SSH, SFTP).

Confirmer le fonctionnement du RAID 6.

S’assurer que Docker et Vaultwarden fonctionnent.

Vérifier le bon démarrage de la VM Debian sous TrueNAS Scale.

10. Conclusion

Ce document détaille les étapes d’installation et de configuration d’un serveur TrueNAS Scale, en assurant une infrastructure fonctionnelle et sécurisée pour le stockage et les services réseau.

