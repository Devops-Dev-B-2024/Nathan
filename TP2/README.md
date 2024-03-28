## Commandes de Nathan OGER

#### Exécuter un serveur web (apache, nginx, ...) dans un container docker

#### Récupérer l'image de Nginx depuis le Dockerhub 
Elle est disponible içi : https://hub.docker.com/_/nginx

#### Utilisez une commande pour vérifier que vous disposez bien de l'image en local :




ESPACCCCCCEs

#### Avec un fichier Dockerfile :

J'ai lancé la commande suivante :

```
docker build -t tp2:1.0 . 
```
Avec le fichier Dockerfile suivant :

```
FROM nginx
COPY ./index.html /usr/share/nginx/html/index.html
```