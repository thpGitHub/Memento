Express JS
-
> Express.js est un framework minimaliste pour node.js.

Pour ajouter Express dans le dossier de votre projet :
```shell script
   $ npm i express
```
> cette commande va créér un dossier ``node_modules`` un fichier ``package-lock.json`` et enregistrer ``express``
> en tant que dépendance dans le fichier ``package.json`` (``package.json`` a été créé par ``$ npm init``).
> ***``Attention``*** il faut avant d'installer express faire un ``$ npm init``.


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
    // const port = process.env.PORT || 8000;

    // ou const app = require('express') ();
    // app.get(path, callback [, callback ...])
    // requête GET vers le chemin demandé
    app.get('/', function (req, res) {
      res.send('hello world');
    })
    app.listen(3000);
/*
    http.listen(port, () => {
        const date = new Date();
        console.log(`${ date.getHours() }H${ date.getMinutes() } on port : ${ port }`);
    });
*/
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

Deux façon de créer un serveur HTTP :
````javascript
    // 1: création d'un serveur HTTP par express
    const express = require('express');
    const app = express();
    
    //...
    
    app.listen(3000);

    // 2: création d'un seveur HTTP manuellement
    // cette méthode est pratique pour exécuter socket.io dans la même instance de serveur HTTP
    const express = require('express');
    const http = require('http'); 
    const app = express();
    const server = http.createServer(app);
    
    //...
    
    server.listen(3000);
````

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

autre solution : Installer le package ``cors``
````shell script
    $ npm i cors
````
usage simple :
> ici Faire appel à ce package sans lui passer d’arguments permet d’autoriser tous les accès à votre ressource.

````javascript
    var express = require('express')
    var cors = require('cors')
    var app = express()
     
    app.use(cors())
     
    app.get('/products/:id', function (req, res, next) {
      res.json({msg: 'This is CORS-enabled for all origins!'})
    })
     
    app.listen(80, function () {
      console.log('CORS-enabled web server listening on port 80')
    })

````

---

### Les routes POST (***attention*** voir plus bas pour modifier ``Récupérer les données d'une requête post``)

> Un package utile quand on veut gérer des demandes POST est : ``body-parser``
````shell script
    $ npm i body-parser
````
Analysez les corps des requêtes entrantes dans un middleware avant vos gestionnaires,
 disponibles sous la req.bodypropriété.
 ````javascript
    const bodyParser = require('body-parser');
    app.use(body_parser.urlencoded({ extended: false }));
    //....
    //app.use(bodyParser.json());
    
    app.post('/', (req, res, next) => {
       console.log(req.body);
    });

````

> Voici un petit exemple de redirection qui fonctionne avec tous les types de route 
> ``res.render()``
````javascript
if (!identifiant || !mdp) {
        res.redirect('/admin');
        return; // return arrête immédiatement la route et la suite du code n'est pas exécuté !!!!!!!
    }

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
    app.use('/js', express.static(__dirname + '/public/javascript'));
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
        // passer la session à la page :
        // res.render('accueil',{title: 'Accueil du site',session:req.session});
    });
    /*
    console.log(req.session): = 
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
> Le dossier ``includes`` contenu dans un dossier ``views`` contiendra nos templates comme par exemple :

- accueil.pug
````jade
    include includes/head
    include includes/header
    section
        h2 Page d'accueil
    include includes/footer
````

- head.pug
````jade
    doctype html
    head
        title= title
        link(rel="stylesheet", href="/src/style_index.css")
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
    const MongoClient = require('mongodb').MongoClient,
          url         = 'mongodb://localhost:27017/',
          dbName      = 'paris';
    
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

### Se connecter à une base de donnée MongoDB avec le module : ``mongodb`` et avec un fichier js externe 
- fichier : db.js :
````javascript
    const MongoClient = require('mongodb').MongoClient,
          url         = 'mongodb://localhost:27017/',
          dbName      = 'blog';
    
    exports.connectDB = (cb) => {
        MongoClient.connect(url,{ useNewUrlParser:true, useUnifiedTopology:true }, (err, client) => {
            if (err) {
                return console.log('err');
            }
            const theDB = client.db(dbName);
            cb(theDB, client);
        });
    };
````
- fichier : app.js
````javascript
    const execDB = require('./db');
    // ici exemple avec une route post :
    app.post('/admin/verification', (req, res) => {
       // ...
       // ...
    
        execDB.connectDB((theDB, client) => {
                const myCollection = theDB.collection('utilisateurs');
                myCollection.find({ identifiant: identifiant, mdp: mdp }).toArray((err, docs) => {
                    client.close();

                   // ici un exemple simple de test du résultat de la requête : 
                  /*  if (docs.length) {
                        res.render('confirmation', { title: 'Page de confirmation', message: 'cool' });
                    }
                    else {
                        res.render('admin', { title: 'Page admin', message: 'pas cool', session: req.session });
                    }*/
                });
            }
        )  
    });
````
### Amélioration du code 
- fichier : db.js
````javascript
    const MongoClient = require('mongodb').MongoClient,
        url         = 'mongodb://localhost:27017/',
        dbName      = 'blog_Ifocop';
    
    const connectDB = (cb) => {
        MongoClient.connect(url,{ useNewUrlParser:true, useUnifiedTopology:true }, (err, client) => {
            if (err) {
                return console.log('err');
            }
            const theDB = client.db(dbName);
            cb(theDB, client);
        });
    }

    exports.find = (settings) => {
        connectDB((theDB, client) => {
            const myCollection = theDB.collection(settings.theCollection);
            myCollection.find(settings.filter).toArray((err, data) => {
                client.close();
                settings.done(data);
            });
        });
    };

    exports.insert = (settings) => {
        connectDB((theDB, client) => {
            const myCollection = theDB.collection(settings.theCollection);
            myCollection.insert(settings.values,(err) => {
                client.close();
                settings.done();
            });
        });
    };
````

- fichier : app.js :
````javascript
    const execDB = require('./db');
        // ici exemple avec une route post :
        app.post('/admin/verification', (req, res) => {
           // ...
           // ...
        
            execDB.find({
                    theCollection: 'utilisateurs',
                    filter: { identifiant: identifiant, mdp: mdp },
                    done: (data) => {
                        if (data.length) {
                            res.render('confirmation', { title: 'Page de confirmation TESSST', message: 'eheheheeheh' });
                        }
                        else {
                            res.render('admin', { title: 'Page admin TESSSST', message: 'ohhhhhhh', session: req.session });
                        }
                    }
            });
        });
        // une autre route pour insérer des données
        app.post('/admin/ajouter_article', (req, res) => {
        
            if (req.body.titre) {
                execDB.insert({
                    theCollection: 'articles',
                    values: { titre: req.body.titre, contenu: req.body.contenu , auteur: session.utilisateur, date: Date.now() },
                    done: () => {
                        res.render('admin', { title: 'Page admin', message: 'article inséré', session: req.session });
                    }
                });
            } else {
                res.redirect('/admin/ajouter_article');
                console.log('redirection ok');
            }
        
        });
````

### ObjectId
> Pour pouvoir utiliser l' ``_id`` d'une base mongoDb il faut utiliser l'objet ``OjectId`` : 
- fichier db.js : 

````javascript
    const ObjectId = require('mongodb').ObjectId;

    // on peut créer une methode a exporter pour utiliser l'objet :
    exports.getObjectId = (idText) => {
        return new ObjectId(idText);
    };
````
- fichier app.js : 

````javascript
    const execDB = require('./db');

    app.get('/admin/article_modifier', (req, res) => {
        //console.log(req.query);
        const theId = execDB.getObjectId(req.query.id);
        execDB.find({
            theCollection: 'articles',
            filter: { _id: theId },
            done: (data) => {
                console.log(data);
                res.send('ok' ) ;
            }
        });
    });
````
 ---
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
### Création d'un fichier ``router.js``

- fichier ``app.js``
````javascript
    const router = require('./router');

    app.use(router); // Requests processing will be defined in the file router
````
- fichier ``router.js``
````javascript
    const express = require("express");
    const router = express.Router();
    
    module.exports = router;

    router.get("/", (req, res) => {
           res.json("Hello world!!");
       });
````
// http://www.cril.univ-artois.fr/~boussemart/express/chapter01.html

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
    │   ├── app.js
    │   └── users.js
    └── views
        ├── error.pug
        ├── index.pug
        └── layout.pug




````


### A voir  express-validator
- https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs/forms
