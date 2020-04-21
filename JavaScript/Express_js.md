Express JS
-

Pour ajouter Express dans le dossier de votre projet :
```shell script
   $ npm i express
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
- ``.use()`` traitera toutes les requêtes.
- ``.get()`` ne traitera que les requêtes de type GET.
- ``.post()`` ne traitera que les requêtes de type POST.

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

> Un package utile quand on veut gérer des demandes POST est : ``body-parser``
````shell script
    $ npm install body-parser
````
Analysez les corps des requêtes entrantes dans un middleware avant vos gestionnaires,
 disponibles sous la req.bodypropriété.
 ````javascript
    const bodyParser = require('body-parser');
    //....
    app.use(bodyParser.json());
    
    app.post('/', (req, res, next) => {
       console.log(req.body);
    });

````
### Les fichiers statics
> Les fichiers statiques sont les images, les fichiers CSS et les fichiers JavaScript...   

````javascript
    app.use(express.static('public'));
````
> la fonction ``express.static`` est a utiliser dans une fonction ``middleware``
````javascript
    app.use('/static', express.static('public'));
````

````javascript
    app.use('/fichier', express.static(__dirname + '/stock'));
        
    app.get('/', (req, res)=> {
        res.send('<img src="/fichier/burger.jpg" alt="burger img">');
    });
````
> la methode ``res.sendfile()``
> La méthode sendFile de l'objet res permet d'envoyer un fichier spécifique au client.
````javascript
    app.get('/', (req, res) => {
        res.sendFile('accueil.html', { root: 'pages' }); 
        // envoie le fichier accueil.html qui se trouve dans le dossier racine page
    });
````



### Templates
> Le moteur de template le plus utilisé avec express js est ``pug``

````shell script
    $ npm i pug
````
````javascript
    // utiliser le module pug
    app.set('view engine', 'pug');
    // les fichiers statics ``pug`` se trouverons dans le dossier : /views
    app.set('views', './views');
````
````html
    // le fichier index.pug dans le dossier views
    html
      head
        title= title
      body
        h1= message
````
````javascript
    app.get('/', function (req, res) {
      res.render('index', { title: 'Hey', message: 'Hello there!'});
    });
````


### Mongoose
> Mongoose est un package qui facilite les interactions avec notre base de données MongoDB grâce à des fonctions extrêmement utiles.
````shell script
    $ npm i mongoose
````
````javascript
    const mongoose = require('mongoose');

    mongoose.connect('mongodb+srv://thp_adm:<password>@cluster0-q8dcp.mongodb.net/test?retryWrites=true&w=majority',
      { useNewUrlParser: true,
        useUnifiedTopology: true })
      .then(() => console.log('Connexion à MongoDB réussie !'))
      .catch(() => console.log('Connexion à MongoDB échouée !'));
````
> L'un des avantages que nous avons à utiliser Mongoose pour gérer notre base de données MongoDB est que nous pouvons implémenter des schémas de données stricts