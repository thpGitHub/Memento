# Deploying MERN Stack to Heroku

Le projet doit avoir son propre référentiel git (dans le dossier racine du projet).

La Structure du projet doit contenir à sa  racine tous les dossiers liés au serveur (models, routes, package.json,node_modules, .gitignore...), ainsi qu'un dossier (client) pour contenir l'application React (node_module, .gitignore, src, public, package.json ...).

|__client/  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|__node_modules/  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|__public/  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|__src/  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|.gitignore  
|__models/  
|__node_modules  
|__routes/  
|__.gitignore  
|__package.json  
|__server.js  

### Express : server.js

Configuration de Express pour faire deux tâches, il gèrera les appels d'API comme avant et il servira également notre application React

````javascript
// ... other imports 
const path = require("path")

// ... other app.use middleware 
app.use(express.static(path.join(__dirname, "client", "build")))

// ...
// Right before your app.listen(), add this:
app.get("*", (req, res) => {
    res.sendFile(path.join(__dirname, "client", "build", "index.html"));
});

app.listen(...)
````

`app.get("*")` est un gestionnaire de route fourre-tout. Il doit être le plus bas possible dans le fichier server.js afin qu'il soit activé que si les routes API au dessus ne gèrent pas la demande. Il se chargera de renvoyer le fichier principal index.html au client.  

### Ajouter dans le fichier `package.json` du dossier client (React)

````json
"proxy": "http://localhost:8000"
````

### Ajouter un `.gitignore` à la racine du projet

````text
.DS_Store
node_modules/
.env
*.orig
.idea/
.vscode/
````

### Dire à Heroku de compiler (build) et d'exécuter notre application après le déploiement

Dans le fichier `.gitignore` de react nous ignorons le dossier `build`.  
Heroku recherchera dans `package.json` du serveur des scripts à executer

````json
"scripts": {
    "start": "node server.js",
    "heroku-postbuild": "cd client && npm install --only=dev && npm install && npm run build"
}
````

Heroku rechèchera un fichier spécifique pour lui `Procfile` pour lui indiquer comment procéder. Si aucun Procfile n'est trouvé il exécutera `npm start` après le `heroku-postbuild`

### Indiquer à héroku la version de node et de npm utiliser dans notre application

````json
"engines": {
      "node": "12.16.1",
      "npm": "7.6.0"
  }
````  
