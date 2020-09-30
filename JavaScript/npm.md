npm
  -
  - Désinstaller un package
  ````shell script
      $ npm uninstall <package_name>
      $ npm uninstall --save <package_name>
      // si le package a été installé en tant que ``devDependency``
      $ npm uninstall --save-dev <package_name>
  ````
  > Pour vérifier si le package a bien été désinstallé il faut check le dossier ``node_modules``
  > ainsi que le fichier ``package.json``
  ---
  - le module ``dotenv``
  > ce module charge les variables d'environnement à partir d'un fichier ``.env`` 
   A voir : https://github.com/motdotla/dotenv
  ````shell script
      npm i dotenv
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
