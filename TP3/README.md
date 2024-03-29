# Docker TP 3
## Créer un Dockerfile permettant de lancer une application nodeJS

Voilà son contenu :

```
FROM node:20-alpine
WORKDIR ./api
COPY ./YPathe .
RUN rm -rf ./node_modules
RUN npm install
EXPOSE 3000 
```
Puis je lance la commande :

```
docker build -t tp3node .
```

### Créer mon container node depuis mon image

```
docker run -p 3000:3000 -d tp3node
```

### Créer un container de mysql

```
docker run --name TP3Mysql -p 3306:3306 -d mysql
```

### Adapter la connexion à la base de donnée

Il à fallu que je modifie le fichier ```db.config.js``` afin que mon container Node se connecte au container mysql

### Créer un docker compose pour avoir ces deux même services

Note : Node doit se baser sur le build fait plus haut (TP3Node) et mysql sur l'image publique.

Voilà mon docker-compose.yaml :

```` 
name: ypathe
services:
    mysql:
      environment:
        - MYSQL_ROOT_PASSWORD=1234
        - MYSQL_DATABASE=cinema
      image: mysql:latest
      container_name: mysql
      expose:
        - 3306
    node:
      ports:
        - 3000:3000
      image: tp3node
      container_name: node
```` 
J'ai volontairement choisis de ne pas publier le port 3306 de mysql afin de le sécuriser

Mes deux containers ce trouvent bien sur le même network