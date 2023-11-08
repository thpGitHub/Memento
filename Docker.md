# Docker

````shell
docker --version
docker info

docker ps
docker ps -a

docker images
# Supprimera l'image Docker avec le nom
docker rmi my-node-image
# Supprimera l'image Docker avec l'ID
docker rmi abcdef123456


# Télécharge l'image Docker officielle Node.js
docker pull node
docker pull node:14

# Exécutera l'image Node.js en mode interactif c'est à dire avec un terminal
docker run -it node
# Exécutera l'image Node.js en mode détaché c'est à dire en arrière plan
docker run -d node

docker logs 637e25947005

# Arrêtera le container avec l'ID 637e25947005
docker stop 637e25947005

# Supprimera le container avec l'ID 637e25947005
docker rm 637e25947005

# Exécutera un conteneur Docker basé sur l'image officielle NGINX en mode détaché ( -d) et mapper le port 8080 à l'intérieur du conteneur
docker run -d -p 8080:80 nginx
# Exécutera un shell Bash dans un conteneur Docker existant avec l'ID de conteneur spécifié
docker exec -ti 637e25947005 bash


docker system prune

docker volume create mongodb_data
docker volume ls
docker volume inspect mongodb_data
````

## Dockerfile

````dockerfile
FROM node:14

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 8080

CMD [ "node", "server.js" ]
````

`FROM` qui vous permet de définir l'image source ;

`RUN` qui vous permet d’exécuter des commandes dans votre conteneur ;

`ADD` qui vous permet d'ajouter des fichiers dans votre conteneur ;

`WORKDIR` qui vous permet de définir votre répertoire de travail ;

`EXPOSE` qui permet de définir les ports d'écoute par défaut ;

`VOLUME` qui permet de définir les volumes utilisables ;

`CMD` qui permet de définir la commande par défaut lors de l’exécution de vos conteneurs Docker.

````dockerfile
FROM debian:buster-slim

RUN apt-get update -yq \
   && apt-get install -yq curl gnupg \
   && curl -sL https://deb.nodesource.com/setup_10.x | bash - \
   && apt-get install -yq nodejs \
   && apt-get clean -y

COPY . /app/
WORKDIR /app
RUN npm install

EXPOSE 2368

CMD npm run start
````

## .dockerignore

````dockerignore
node_modules
npm-debug.log
.git
````

````shell
# A partir du Dockerfile "."  -t permet de nommer l'image
docker build -t ocr-docker-build .
docker run -d -p 2368:2368 ocr-docker-build
````

---

## Docker compose

````yaml
# docker-compose.yml
version: '3'

services:
  db:
    image: mysql:5.7
    volumes:
      - ./mysql:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: somewordpress
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress

  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    ports:
      - "8000:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress

volumes:
  mysql: {}
````

````shell
# Démarrer les conteneurs Docker en arrière-plan 
docker-compose up -d
# Arrêter les conteneurs Docker
docker-compose stop
# Afficher les conteneurs Docker
docker-compose ps
# Afficher les logs des conteneurs Docker
docker-compose logs
# Détruire les conteneurs Docker
docker-compose down
# Poue valider la configuration de votre fichier docker-compose.yml
docker-compose config
````
