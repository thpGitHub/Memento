npm
-
````shell script
    $ npm init
    $ npm init --yes
````

````shell script
    $ npm install # By default, npm install will install all
    # modules listed as dependencies in package.json.
````
> ``package-lock.json`` est un fichier qui est généré lorsque vous apportez des modifications dans le fichier ``package.json``
> ou dans l'architecture du dossier ``node_modules``. Ce fichier stocke la structure exacte de votre arborescence de dépendances à chaque installation.
> Ce fichier a été conçu spécifiquement pour les projets collaboratifs, il est donc important de bien le commiter. Cela permet de gérer le fait 
> que le dossier ``node_modules`` ne soit pas dans les sources commitées, car Node.js est capable de réinstaller les dépendances de 
> votre projet à partir de ce fichier.
---
  - Désinstaller un package
  ````shell script
      $ npm uninstall <package_name>
      $ npm uninstall --save <package_name>
      # si le package a été installé en tant que ``devDependency``
      $ npm uninstall --save-dev <package_name>
  ````
  > Pour vérifier si le package a bien été désinstallé il faut check le dossier ``node_modules``
  > ainsi que le fichier ``package.json``
  ---
  - le module ``dotenv``
  > ce module charge les variables d'environnement à partir d'un fichier ``.env`` 
   A voir : https://github.com/motdotla/dotenv
  ````shell script
      $ npm i dotenv
  ````
  > Création d'un fichier ``.env``
  ````shell script
      DB_HOST_LOCAL=mongodb://localhost:27017/
      DB_HOST_ATLAS=mongodb+srv://<USER>:<PASS>@cluster0-q8dcp.mongodb.net/questions_for_a_developer?retryWrites=true&w=majority
      DB_USER=<USER>
      DB_PASS=<PASS>
      DB_NAME=<NAME>
  ````
  > Dans ``app.js``
  ````javascript
      require('dotenv').config();
  
      const url = process.env.DB_HOST_LOCAL;
  ````
  ### Ne pas oublier de mettre ``.env`` dans le fichier ``.gitignore``
  
  > Autre technique pour ne pas utiliser de ``require('dotenv').config();``
  >rajouter un ``script`` start dans le ``package.json``
  > voir : https://www.youtube.com/watch?v=hpY72mhpLLk
