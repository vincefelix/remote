# Configuration d'Accès SSH et Gestion des Ports pour une VM dans VirtualBox

Ce guide fournit les étapes pour configurer un accès SSH à une machine virtuelle (VM) dans VirtualBox, rediriger un port pour accéder à la VM depuis l'extérieur, et gérer les règles de pare-feu `ufw` pour sécuriser les connexions.

## Prérequis

- **VirtualBox** avec une VM configurée
- **Accès root** sur la VM
- **Pare-feu `ufw`** activé sur la VM

## Étapes de Configuration

### 1. Configurer la Redirection de Port pour SSH

Comme la VM est isolée derrière un routeur virtuel, ajoutez une redirection de port pour permettre l'accès SSH :

1. **Ouvrir les paramètres de la VM dans VirtualBox**.
2. Aller dans **Réseau** > **Avancé** > **Redirection de ports**.
3. Ajouter une règle de redirection :
   - **Nom** : SSH
   - **Protocole** : TCP
   - **Port Hôte** : (ex. 2222)
   - **Port Invité** : 22

### 2. Connexion SSH

Pour se connecter via SSH :

```bash
ssh -p 2222 root@localhost
```

### 3. Changer le Port SSH
Pour renforcer la sécurité, modifiez le port SSH par défaut :

Modifier le fichier de configuration SSH :

```bash
sudo nano /etc/ssh/sshd_config
```
Changer le port de 22 à un nouveau numéro (ex. 2223) :
plaintext
Port 2223
Redémarrer le service SSH :

```bash
sudo systemctl restart ssh
```
### 4. Mettre à Jour les Règles du Pare-feu ufw
Pour autoriser le nouveau port SSH et bloquer le port par défaut :

Bloquer le port 22 :

```bash
sudo ufw deny 22
```
Autoriser le nouveau port :
```bash
sudo ufw allow 2223/tcp
```	

### 5. Vérification des Règles
Vérifiez que les règles sont appliquées correctement :

```bash
sudo ufw status
```

### 6. Supprimer une Règle de Port du Pare-feu
Pour supprimer une règle de port spécifique :

```bash
sudo ufw delete allow 2222/tcp
```