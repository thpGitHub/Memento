# npm

````shell script
npm outdated # liste les versions des packages installés et les dernières versions disponibles !
````

````shell script
    npm init
    npm init --yes
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

- Installer une version spécifique d'un package

  ````shell script
    $ npm install express@4.16.1
    # avec un signe ^ ou un tilde ~ pour spécifier la dernière version mineure ou patch
    $ npm install express@^4.16.1
    $ npm install express@~4.16.1
  ````

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
   A voir : <https://github.com/motdotla/dotenv>

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

## Ne pas oublier de mettre ``.env`` dans le fichier ``.gitignore``
  
  > Autre technique pour ne pas utiliser de ``require('dotenv').config();``
  >rajouter un ``script`` start dans le ``package.json``
  > voir : <https://www.youtube.com/watch?v=hpY72mhpLLk>

---

## `npm` et `npx`

npm est utilisé pour installer et mettre à jour les paquets déjà installés dans notre projet. npm a la capacité d'exécuter des packages. Si vous utilisez la commande "npm mon-paquet", cela n'exécutera le paquet que s'il a été installé globalement. Vous pouvez donc avoir un message d'erreur pour un paquet installé localement. La solution pour exécuter un paquet local est de saisir le chemin entier vers le paquet. Vous pouvez également utiliser la commande "npm run" pour exécuter un paquet qui a été ajouté à la section "scripts" du fichier "packages.json".

npm est un référentiel en ligne pour la publication de projets Node.js open-source et d'un outil CLI pour nous aider à installer ces packages et à gérer leurs versions et dépendances.

npx (Node Package eXecute) est un outil spécialement conçu pour l'exécution des paquets. Lorsque vous lancez l'exécution d'un paquet avec cet outil, npx va chercher dans la variable "PATH" de l'ordinateur puis dans les fichiers binaires des modules du projet pour lancer la commande. S'il ne l'a pas trouvé, l'outil est même capable d'aller sur internet chercher la commande et de l'exécuter ensuite. Le paquet est exécuté dans le répertoire en cours.

````shell
npx creat-react-app
````

npx exécute des paquets exécutables. Cela signifie que vous allez exécuter le code Create React App sans télécharger le projet au préalable.
Le package exécutable exécutera l'installation de create-react-app dans le répertoire que vous spécifiez.
