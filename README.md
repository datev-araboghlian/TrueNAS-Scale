# Documentation du Projet TrueNAS Scale

## 1. Introduction

TrueNAS Scale est un système d'exploitation NAS (Network Attached Storage) open-source basé sur Debian Linux. Il prend en charge la virtualisation, la conteneurisation (Docker/Kubernetes) et divers protocoles de partage de fichiers tels que CIFS/Samba, NFS et iSCSI. Ce projet implique l'installation de TrueNAS Scale sur une machine virtuelle, la configuration d'un stockage en RAID 6 et la mise en place des connexions SSH, HTTPS et SFTP.

## 2. Installation de TrueNAS Scale

1. Démarrer la VM TrueNAS Scale avec l’ISO monté.
2. Suivre les étapes d'installation :
   - Sélectionner le disque d’installation (16 Go)
   - Configurer le mot de passe administrateur
   - Finaliser l’installation et redémarrer
3. Accéder à l’interface web via l’adresse IP attribuée :

```sh
ip addr show
```

## 3. Configuration du Stockage RAID 6

1. Accéder à **Storage > Pools** et créer un nouveau pool.
2. Ajouter les 5 disques de 2 To et configurer en RAID 6.
3. Nommer le pool `Stockage` et valider.

## 4. Connexion à TrueNAS Scale

- **HTTPS:** Ouvrir un navigateur et entrer `https://<TrueNAS_IP>`.
- **SSH:** Activer SSH dans **System Settings > Services** puis se connecter :

```sh
ssh admin@<TrueNAS_IP>
```

- **SFTP:** Activer et configurer SFTP dans **Services**.

## 5. Activation des Services

1. Aller dans **System Settings > Services**.
2. Activer et configurer SSH et FTP.
3. Modifier le port SFTP (ex: 2222) :

```sh
echo "Port 2222" >> /etc/ssh/sshd_config
systemctl restart ssh
```

## 6. Configuration des Utilisateurs et Sessions SFTP

1. Créer plusieurs utilisateurs avec leurs répertoires dédiés :

```sh
useradd -m -s /bin/bash user1
passwd user1
mkdir /mnt/Stockage/user1
chown user1:user1 /mnt/Stockage/user1
```

2. Définir un dossier public :

```sh
mkdir /mnt/Stockage/Public
chmod 777 /mnt/Stockage/Public
```

3. Tester les connexions SFTP avec un client comme FileZilla.

## 7. Installation de Docker et Vaultwarden

1. Activer Docker :

```sh
systemctl enable docker
systemctl start docker
```

2. Installer Vaultwarden :

```sh
docker run -d --name vaultwarden \
  -v /mnt/Stockage/vaultwarden:/data \
  -p 8080:80 \
  --restart unless-stopped \
  vaultwarden/server:latest
```

3. Vérifier que le service est opérationnel :

```sh
docker ps
```

## 8. Création d'une VM Debian sous TrueNAS Scale

1. Aller dans **Virtual Machines > Add**.
2. Configurer avec les paramètres :
   - **Utilisateur:** `LaPlateforme`
   - **Mot de passe:** `LaPlateforme13`
3. Tester la connexion SSH à cette VM.

## 9. Tests et Validation

- Vérifier l’accès réseau (HTTPS, SSH, SFTP).
- Confirmer le fonctionnement du RAID 6.
- S’assurer que Docker et Vaultwarden fonctionnent.
- Vérifier le bon démarrage de la VM Debian sous TrueNAS Scale.

## 10. Conclusion

Ce document détaille les étapes d’installation et de configuration d’un serveur TrueNAS Scale, en assurant une infrastructure fonctionnelle et sécurisée pour le stockage et les services réseau.

