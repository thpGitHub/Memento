Express JS
-

Pour ajouter Express dans le dossier de votre projet :
```shell script
   $ npm i express
```
> cette commande va créér un dossier ``node_modules`` un fichier ``package-lock.json`` et enregistrer ``express``
> en tant que dépendance dans le fichier ``package.json`` (``package.json`` a été créé par ``$ npm init``).
> Attention il faut avant d'installer express faire un ``$ npm init``.

````json
{
    "dependencies": {
        "express": "^4.17.1"
      }
}
````
Par convention l'objet ``app`` désigne une application ``express`` et possède de nombreuses méthodes   
````javascript
    const express = require('express');
    const app = express();
// ou const app = require('express') ();
    // app.get(path, callback [, callback ...])
    // requête GET vers le chemin demandé
    app.get('/', function (req, res) {
      res.send('hello world');
    })
    app.listen(3000);
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

---

### Les fonctions middleware
>Les fonctions middleware sont des fonctions qui ont accès à l' objet requête ``req``,
> à l' objet réponse ``res`` et à la fonction middleware ``next()``.
````javascript
    const app = express()
    
    app.use(function (req, res, next) {
      console.log('Time:', Date.now());
      next();
    });
````
---

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

---

### Les routes POST

> Un package utile quand on veut gérer des demandes POST est : ``body-parser``
````shell script
    $ npm i body-parser
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

---

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
        // res.sendFile(__dirname + '/exercice1_index.html');
    });
````
---

### Templates
> Le moteur de template le plus utilisé avec express js est ``pug``

````shell script
    $ npm i pug
````
````javascript
    /* utiliser le module pug
       mettre 'pug' permet de dire que les fichiers sont des .pug 
       (on ne sera pas obligé de mettre l'extension du fichier avec la méthode render()
    */
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
---

### Les erreurs
````javascript
    app.use((req, res) => {
        res.status(404).render('error404');
    });
````
````javascript
    app.use((req, res, next) => {
        switch (res.statusCode) {
            case 503:
                res.render('503');
                break;
            default:
                res.status(404).render('404');
        }
    });
````
---

### Les sessions
````shell script
    $ npm i express-session
````
````javascript
    const session = require (' express-session'); 

    app.use(session({
        secret: 'my secret text',
        resave: true,
        saveUninitialized: true,
        cookie: {}
    }));

    app.get('/', (req,res) => {
        req.session.toto = 'ehhhh';
        console.log(req.session);
        res.render('accueil', {title: `Page d'accueil`} );
    });
    /*
    Session {
         cookie: { path: '/', _expires: null, originalMaxAge: null, httpOnly: true },
         toto: 'ehhhh'
     }
    */
````
````javascript
    // supprimer une session
    app.get('/supprimerSession', (req, res) => {
       req.session.destroy((err) => {
           res.render('supprimerSession', {title: 'session supprimée'});
       });
    });
````

> Les données de session sont stockées côté serveur.   
> Depuis la version 1.5.0, le cookie-parser n'a plus besoin d'être utilisé pour que ce module fonctionne.

---

### includes 
> Le dossier ``includes`` contenu dans un dossier ``views`` contiendra nos templates comme par exemple : :
 - head.pug
````jade
    doctype html
    head
        title= title
        link(rel="stylesheet", href="/src/style.css")
````

- hearder.pug
````jade
    body
        header
            h1= title
            nav
                ul
                    li
                        a(href="/") Accueil
                    li
                        a(href="/admin") Administration
                    if session && session.identifiant
                        li
                            a(href="/admin/quitter") Se déconnecter
            if message
                p#message= message
````

- footer.pug
````jade
    // ici code du footer ;)
````
---

### extends  

---

### Récupérer les données dans l'URL (get)
> L'objet `req` a deux propriétées pour récupérer les données passées dans l'URL 
- ``params``   
````javascript
    // pour l'URL : http://www.monsite.com/infos/truc/machin
    app.get('/infos/:un/:deux', (req,res) => {
      console.log(req.params.un); //affiche truc 
      console.log(req.params.deux); //affiche machin
    });
````   

- ``query`` 
````javascript
    // pour l'URL : http://www.monsite.com/question?r=chose&t=bidule
    app.get('/question', (req,res) => {
        console.log(req.query.r); //affiche chose
        console.log(req.query.t); //affiche bidule
    });
````

### Récupérer les données d'une requête post
>Afin de pouvoir récupérer les informations du corp de la requête POST il faut instatller le package : ``body_parser``
````shell script
    $ npm i body-parser
````
````javascript
    const bodyParser = require('body-parser');

    app.use(bodyParser.urlencoded({ extended: false }));

    app.post('/traitement', (req, res) => {
        res.render('resultat',{datas:req.body}); 
    });
````
---

### Se connecter à une base de donnée MongoDB avec le module : ``mongodb``
https://expressjs.com/en/guide/database-integration.html#mongodb
https://www.npmjs.com/package/mongodb
// choir le driver de notre mongoDB :   
http://mongodb.github.io/node-mongodb-native/ 
http://mongodb.github.io/node-mongodb-native/3.5/quick-start/quick-start/

````shell script
    $ npm i mongodb
````
````javascript
    const MongoClient = require('mongodb').MongoClient;
    
    app.get('/', (req,res) => {
        MongoClient.connect(url,{useNewUrlParser:true, useUnifiedTopology:true}, (err, client) => {
            if (err) {
                return console.log('err');
            }
            const db = client.db(dbName);
            const myCollection = db.collection('piscines');
            myCollection.find({}).toArray((err, docs) => {
                client.close();
                console.log(docs);
                res.render('accueil', {title: 'Accueil', datas: docs});
            });
        });
    });

// p= datas[0].zipCode
````
````jade
    // accueil.pug
    doctype html
    html
        head
            title= title
        body
            h1 Bienvenue sur la page d'accueil Exo 16          
        section
            ul
                each piscines in datas
                    li #{piscines.name} #{piscines.zipCode}

// p= datas[0].zipCode
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

---

### Exemple d'une structure d'application Express 
````text
    .
    ├── app.js
    ├── bin
    │   └── www
    ├── package.json
    ├── public
    │   ├── images
    │   ├── javascripts
    │   └── stylesheets
    │       └── style.css
    ├── routes
    │   ├── index.js
    │   └── users.js
    └── views
        ├── error.pug
        ├── index.pug
        └── layout.pug




````
