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
docker rmi -f c74c862bce75


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
docker system prune -a

docker volume create mongodb_data
docker volume ls
docker volume inspect mongodb_data

docker inspect <container-id-or-name> | grep IPAddress
docker inspect <container-id-or-name> | Select-String -Pattern 'IPAddress'

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

````shell
# A partir du répertoire ou se trouve le Dockerfile "."  -t permet de nommer l'image
docker build -t ocr-docker-build .
docker run -d -p 2368:2368 ocr-docker-build

# Exemple react
docker build -t my-app .
# On mappe le port 3000 du conteneur sur le port 3001 de la machine hôte.
# car le port par default pour un application react est le port 3000.
docker run -p 3001:3000 my-app

# Exécutera un shell Bash dans un conteneur Docker existant avec l'ID de conteneur spécifié
docker exec -ti 637e25947005 bash
# exemple avec mongoDB
# une fois connecté au container mongoDB on peut lancer la commande mongoh pour se connecter à la base de données
> mongosh
> show dbs
> use mydb
> show collections
> db.users.find()
> db.myCollection.insertOne( { x: 1 } )



````

## .dockerignore

````dockerignore
node_modules
npm-debug.log
.git
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

````shell
version: '3'
services:
  node:
    build:
      context: ./back
    ports:
      - "3000:3000"
    depends_on:
      - mongo
  mongo:
    build:
      context: ./bdd
    ports:
      - "27017:27017"
    volumes:
      - ./mongo_data:/data/db
volumes:
  mongo_data:      
````

````shell
# Use the official MongoDB image as the base image
FROM mongo:latest

# Ajoute un script de configuration
COPY mongo-init.js /docker-entrypoint-initdb.d/

# Exécute le serveur MongoDB
CMD ["mongod"]

# Expose the default MongoDB port
EXPOSE 27017
````

````javascript
// mongo-init.js

db = db.getSiblingDB('quiz_bdd_in_docker');

db.users.insertOne({
    "email": "thierrypoupon@hotmail.com",
    "password": "$2b$10$2vKoP04xSDJi25/ovra.L..uKt5.lWGZJMS3aD/E5uUHxwIpSRF2a",
});

db.quizzes.insertMany([
    {
        "name": "Geography Quiz",
        "rounds": [
          {
            "questions": "What is the capital of Italy?",
            "reponses": ["Rome", "London"],
            "corrects": [0]
          },
          {
            "questions": "Which river flows through Egypt?",
            "reponses": ["Nile", "Amazon"],
            "corrects": [0]
          }
        ],
        "categories": ["Geography", "World Capitals"],
    },
    {
        "name": "Movie Quotes Quiz",
        "rounds": [
          {
            "questions": "Which movie features the line 'You can't handle the truth!'?",
            "reponses": ["A Few Good Men", "The Shawshank Redemption"],
            "corrects": [0],
          },
          {
            "questions": "In which film does the character say 'Here's looking at you, kid'?",
            "reponses": ["Casablanca", "Gone with the Wind"],
            "corrects": [0],
          }
        ],
        "categories": ["Movies", "Famous Quotes"],
      },
      {
        "name": "History Quiz",
        "rounds": [
          {
            "questions": "Which year did World War II end?",
            "reponses": ["1945", "1918"],
            "corrects": [0],
          },
          {
            "questions": "Who was the first president of the United States?",
            "reponses": ["Thomas Jefferson", "George Washington"],
            "corrects": [1],
          }
        ],
        "categories": ["History", "General Knowledge"],
      },
      {
        "name": "Science Quiz",
        "rounds": [
          {
            "questions": "What is the chemical symbol for water?",
            "reponses": ["H2O", "CO2"],
            "corrects": [0],
          },
          {
            "questions": "What is the closest planet to the sun?",
            "reponses": ["Venus", "Mercury"],
            "corrects": [1],
          }
        ],
        "categories": ["Science", "General Knowledge"],
      },
      {
        "name": "Math Quiz",
        "rounds": [
          {
            "questions": "What is 2 + 2?",
            "reponses": ["3", "4"],
            "corrects": [1],
          },
          {
            "questions": "What is the value of π (pi)?",
            "reponses": ["3.14", "3.14159"],
            "corrects": [1],
          }
        ],
        "categories": ["Mathematics", "General Knowledge"],
      },
      {
        "name": "Music Quiz",
        "rounds": [
          {
            "questions": "Who is known as the 'King of Pop'?",
            "reponses": ["Elvis Presley", "Michael Jackson"],
            "corrects": [1],
          },
          {
            "questions": "Which band performed the hit song 'Bohemian Rhapsody'?",
            "reponses": ["The Beatles", "Queen"],
            "corrects": [1],
          }
        ],
        "categories": ["Music", "Entertainment"],
      },
      {
        "name": "Sports Quiz",
        "rounds": [
          {
            "questions": "Which sport does Cristiano Ronaldo play?",
            "reponses": ["Basketball", "Soccer"],
            "corrects": [1],
          },
          {
            "questions": "In which sport is the 'Wimbledon' championship held?",
            "reponses": ["Tennis", "Golf"],
            "corrects": [0],
          }
        ],
        "categories": ["Sports", "General Knowledge"],
      },
      {
        "name": "Language Quiz",
        "rounds": [
          {
            "questions": "Which language is spoken in Japan?",
            "reponses": ["Chinese", "Japanese"],
            "corrects": [1],
          },
          {
            "questions": "What is the official language of Brazil?",
            "reponses": ["Portuguese", "Spanish"],
            "corrects": [0],
          }
        ],
        "categories": ["Languages", "Culture"],
      },
      {
        "name": "Food Quiz",
        "rounds": [
          {
            "questions": "Which country is famous for sushi?",
            "reponses": ["Italy", "Japan"],
            "corrects": [1],
          },
          {
            "questions": "What is the main ingredient in guacamole?",
            "reponses": ["Avocado", "Tomato"],
            "corrects": [0],
          }
        ],
        "categories": ["Food", "Cuisine"],
      }
]);
````

## Annexes

### Dockerfile mysql

````dockerfile
# use mysql:latest as base image
FROM mysql:latest

#set env variables
ENV MYSQL_DATABASE=ynov_app
ENV MYSQL_ROOT_PASSWORD=root

#copy sql script to docker-entrypoint-initdb.d
COPY ./sql-scripts/ /docker-entrypoint-initdb.d/

#build an image on docker 
#docker build /dossier_source -t nom_image
#run an image on docker
#docker run -d -p 3306:3306 --name nom_container nom_image

# Dans le container MySQL de docker Desktop, on peut se connecter à la base de données.On peut accéder au bash du container MySQL dans l'onglet "Exec" de docker Desktop.
# On peut se connecter à la base de données avec la commande mysql -u root -p
# On peut voir les bases de données avec la commande show databases;
# On peut sélectionner une base de données avec la commande use ynov_app;
# On peut voir les tables de la base de données avec la commande show tables;
# On peut voir le contenu d'une table avec la commande select * from users;
````
