# Linux - Commandes essentielles

## Navigation et fichiers

```bash
pwd                     # Repertoire courant
ls -la                  # Lister avec details et fichiers caches
cd /path/to/dir         # Changer de repertoire
find / -name "*.log"    # Chercher des fichiers
du -sh /var/log         # Taille d'un repertoire
df -h                   # Espace disque
```

## Permissions

```bash
chmod 755 script.sh     # rwxr-xr-x (proprio: tout, groupe/autres: lire+exec)
chmod 600 secret.key    # rw------- (proprio seul)
chown user:group file   # Changer proprietaire
```

| Chiffre | Permission |
|---------|-----------|
| 7 | rwx (lire + ecrire + executer) |
| 6 | rw- (lire + ecrire) |
| 5 | r-x (lire + executer) |
| 4 | r-- (lire seul) |
| 0 | --- (rien) |

## Gestion des processus

```bash
ps aux                  # Tous les processus
top / htop              # Monitoring temps reel
kill <PID>              # Arreter un processus (SIGTERM)
kill -9 <PID>           # Forcer l'arret (SIGKILL)
nohup command &         # Lancer en arriere-plan (survit au logout)
```

## Services (systemd)

```bash
systemctl start nginx       # Demarrer
systemctl stop nginx        # Arreter
systemctl restart nginx     # Redemarrer
systemctl status nginx      # Statut
systemctl enable nginx      # Lancer au demarrage
journalctl -u nginx -f      # Logs en temps reel
```

## Gestion des utilisateurs

```bash
useradd -m username         # Creer un utilisateur avec home
passwd username             # Definir le mot de passe
usermod -aG docker username # Ajouter a un groupe
su - username               # Changer d'utilisateur
```

## Reseau

```bash
ip addr                 # Interfaces reseau
netstat -tlnp           # Ports en ecoute
curl -v http://host     # Requete HTTP avec details
ssh user@host           # Connexion SSH
scp file user@host:/path # Copier un fichier via SSH
```

## Gestion des paquets (yum / apt)

```bash
# Amazon Linux / CentOS (yum)
yum update -y
yum install -y package-name
yum list installed

# Ubuntu / Debian (apt)
apt update && apt upgrade -y
apt install -y package-name
apt list --installed
```
