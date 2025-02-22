# Nextcloud

Installation de Nextcloud en **Docker Compose**, avec:
     
-**PHP-FPM** pour accélérer l'affichage.

-**MariaDB** externe pour la persistance des données.

-**Redis** pour la gestion des sessions et du cache.


Je déconseille l'utilisation d'un conteneur **lxc non privilégié** si vous comptez utiliser du stockage externe.

## Ressources serveur

- **Processeur** : 6 cœurs
- **Mémoire RAM** : 8 Go
- **Espace disque** : Minimum 40 Go (recommandé SSD pour de meilleures performances)

## Installation sur Debian

**Installation docker compose:**

```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
**Installation Nextcloud**

```bash
#Après avoir téléchargé le docker-compose
sudo docker compose up -d
```
Vider logs: truncate -s 0 /var/lib/docker/volumes/nextcloud_nextcloud/_data/data/nextcloud.log
sudo docker exec -u 33 -it e79 php occ maintenance:repair --include-expensive
sudo docker exec -u 33 -it e79 php occ config:system:set maintenance_window_start --type=integer --value=1
sudo docker exec -u 33 -it e79 php occ db:add-missing-indices
