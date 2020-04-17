Express JS
-

Pour ajouter Express dans le dossier de votre projet :
```shell script
   $ npm i express --save
```
> cette commande va créér un dossier ``node_modules`` un fichier ``package-lock.json`` et enregistrer ``express`` en tant que dépendance dans le fichier ``package.json``

````json
{
    "dependencies": {
        "express": "^4.17.1"
      }
}
````
Par convention l'objet ``app`` désigne une application ``express`` et possède de nombreuses méthodes   
````javascript
    const express = require('express')
    const app = express()
    // app.get(path, callback [, callback ...])
    // requête GET vers le chemin demandé
    app.get('/', function (req, res) {
      res.send('hello world')
    })
    app.listen(3000)
````

````javascript
    // si on ne spécifie pas le path, le path par default est '/'
    app.use(function (req, res) {
      //...
    });
````
### Les fonctions middleware
>Les fonctions middleware sont des fonctions qui ont accès à l' objet requête ``req``,
> à l' objet réponse ``res`` et à la fonction middleware ``next()``.
````javascript
    const app = express()
    
    app.use(function (req, res, next) {
      console.log('Time:', Date.now())
      next()
    });
````
### Erreurs de CORS 
> Cross Origin Resource Sharing est un système de sécurité qui, par défaut
> bloque les appels HTTP effectués entre des serveurs différents.
>Pour résoudre le problème nous devons ajouter des headers à notre objet response (res).
````javascript
    app.use((req, res, next) => {
        //Permet d'accéder à notre API depuis n'importe quelle origine ('*')
       res.setHeader('Access-Control-Allow-Origin', '*');
        //Permet d'ajouter les headers mentionnés aux requêtes envoyées vers notre API (Origin , X-Requested-With , etc.) ;
       res.setHeader('Access-Control-Allow-Headers', 'Origin, X-Requested-With, Content, Accept, Content-Type, Authorization');
        //Permet d'envoyer des requêtes avec les méthodes mentionnées ( GET ,POST , etc.).
       res.setHeader('Access-Control-Allow-Methods', 'GET, POST, PUT, DELETE, PATCH, OPTIONS');
        // Permet de passer au middleware suivant 
       next();
    });
````

### Les routes POST