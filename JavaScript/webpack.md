WEBPACK
-

````shell script
    $ npm install webpack webpack-cli --save-dev
````

Création d'un fichier : ``webpack.config.js``

````javascript
    module.exports =  {
        entry: './src/index.js',
        mode: 'development',
        output: {
            filename: 'bundle.js',
            path: 'C:\\Users\\thier\\OneDrive\\Documents\\AJAX\\AJAX_RXJS_webpack_npm\\dist',
            // on peut aussi utiliser le module path pour gérer les paths plus facilement.
            // le path du fichier de sortie doit être obligatoirement absolu
        },
    };
````
Dans le fichier ``index.html`` :
````html
    <body>
        <script src="./dist/bundle.js"></script>
    </body>
````

> Il y a deux manières de lancer webpack :
- Dans le terminal :
````shell script
    $ node_modules\.bin\webpack
    # exécution de webpack se trouvant dans le dossier node_modules...
    # du projet
````
Si on veux que webpack se relance automatiquement :
````shell script
    $ node_modules\.bin\webpack --watch
````
- Ajouter un script dans le fichier ```package.json ``` du projet :   
Nous allons créer deux scripts ``build`` et ``dev`` pour lancer les deux commandes du terminal ci-dessus :   
````javascript
    "scripts": {
        "build": "webpack",
        "dev": "webpack --watch"
      },
````
Executer les scripts dans le terminal : 
````shell script
    $ npm run build
    $ npm run dev
````