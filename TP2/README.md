# Commandes de Nathan OGER

## Exécuter un serveur web dans un container via les commandes uniquement

### Récupérer l'image de Nginx depuis le Dockerhub 
Elle est disponible içi : https://hub.docker.com/_/nginx
il faut ensuite executer la commande ``` docker pull nginx ```.

### Utilisez une commande pour vérifier que vous disposez bien de l'image en local :

La commande utilisé est : ``` docker images ```.

Et voici le resultat : 

REPOSITORY  |  TAG    |   IMAGE ID   |    CREATED    |    SIZE
|-----------------|---------|----------------|------------------|--------
yform-front |  latest  |  3bc1e54a5729 |  17 hours ago |  168MB
yform    |     latest  |  6cc40449e7ea  | 17 hours ago  | 187MB
nginx    |     latest  |  92b11f67642b   | 6 weeks ago |   187MB

Pour une raison que j'ignore, l'image nginx que je viens de pull, à été créer depuis 6 semaines... (?)

### Démarrer un nouveau container et servir une page html

J'ai utilisé la commande suivante afin de créer mon container: 
```
docker run --name nginxTP2 -v .\html\:/usr/share/nginx/html:ro -p 80:80 -d nginx
```
Il fonctionne bel et bien

### Supprimer le container

```
docker stop 28efe0468b26
```

Puis 

```
docker rm 28efe0468b26
```

### Relancer le container, mais sans -v

``` 
docker run --name nginxTP2 -p 80:80 -d nginx
```
Puis j'utilise la commande ```docker cp``` pour déplacer mon dossier html dans mon container

```
docker cp .\html\ nginxTP2:/usr/share/nginx/ 
```

## Exécuter un serveur web avec un fichier Dockerfile :

J'ai lancé la commande suivante :

```
docker build -t tp2:1.0 . 
```

Avec un fichier Dockerfile contenant ce qui suit :

```
FROM nginx
```

### Exécuter cette nouvelle image de manière à desservir index.html

Seul le Dockerfile change :
```
FROM nginx
COPY ./html /usr/share/nginx/
EXPOSE 80
```
Puis pour run l'image :

```
docker run -d -p 80:80 tp2:1.0 
```

### Quelles différences observez-vous entre utiliser les commandes et le Dockerfile

Je trouve personnalement plus partique le fait d'utiliser un Dockerfile, en revenche je pense qu'en production, il est plus pratique d'automatiser les commandes.

## Utiliser une base de donnée dans un container Docker

Afin de créer un container mysql, j'éxécute les commandes : 
```
docker pull mysql
```
Puis
```
docker run -d -p 3306:3306 --name mysql -e MYSQL_ROOT_PASSWORD=1234 mysql
```

### Mysql est pret, il ne manque plus que phpmyadmin
Télécharger l'image de phpmyadmin :
```
docker pull phpmyadmin
```
Puis la run : 
```
docker run -d -p 8080:80 --link mysql:db --name phpmyadmin -e MYSQL_ROOT_PASSWORD=1234 phpmyadmin
```

Phpmyadmin & mysql fonctionnent correctement !

## Utilisation de Docker-compose

La principale différence avec les méthodes précédemment utilisés est l'utilisation du fichier YAML.

### Les commandes pour lancer et couper tout les container
Les commandes sont :
``` docker compose up ``` et ``` docker compose down ```


### Ecrivez un fichier docker-compose.yaml pour mettre en service mysql et phpmyadmin

```
name: server
services:
    mysql:
        ports:
            - 3306:3306
        container_name: mysql
        environment:
            - MYSQL_ROOT_PASSWORD=1234
        image: mysql
    phpmyadmin:
        ports:
            - 8080:80
        links:
            - mysql:db
        container_name: phpmyadmin
        environment:
            - MYSQL_ROOT_PASSWORD=1234
        image: phpmyadmin
```

Puis j'éxécute la commande :

```
docker compose up
```

Et celà fonctionne !