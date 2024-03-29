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

## Questions : 

### 1 :  Que se passe t'il si un de mes ports publiés est déjà utilisé ?

Dans ce cas, la seconde application à se lancer, qui à besoin du port ne va pas se lancer, et se mettre en erreur.

### 2 : Quelle option de la commande npm install permet de n'installer que les dépendances de production

Il s'agit de la commande ````npm i --only=prod````

### 2 bis : pourquoi faire cela ?
Cette commande permettera de ne pas télécharger les ````DevDependencies```` qui ne sont utiles qu'aux développeur, ce paramètre ne sera utilisé qu'en production donc.

### 3 : Comment peut-on analyser la sécurité d'une application comme celle-ci (dépendances & image docker)

Le principale facteur de sécurité sera la publication des ports, s'ils sont simplement exposé, ils ne sont disponible qu'avec les autres container, donc par le développeur ou l'organisation.

### 4 : Pourquoi à l'étape 6 mon container node n'arrive pas à communiquer avec ma base de données si je laisse "localhost" en hostname ?

Si ````localhost```` est renseigné, alors le container va tempter de se connecter à lui même, c'est pourquoi à l'étape 6, (donc dans le cas ou j'ai deux conteneurs non networké) j'avais renseigné ````host.docker.internal````