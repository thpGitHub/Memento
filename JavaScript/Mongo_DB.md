MongoDB
-
> Après avoir installé mongoDB (attention pour l'instant je suis dans : C:\Program Files\MongoDB\Server\4.2\bin)
> on peut commencer par vérifier les services windows qui sont sur notre machine
> dans recherche taper simplement ``services`` et le service se nomme : ``mongoDB Server``


````shell script
    # lancer dans le répertoire ou se trouve mongo.exe
    $ mongo
````
>mongo est une interface shell JavaScript interactive pour MongoDB,
>qui fournit une interface puissante pour les administrateurs système ainsi 
>qu'un moyen pour les développeurs de tester les requêtes et les opérations directement 
>avec la base de données. mongo fournit également un environnement JavaScript
> entièrement fonctionnel à utiliser avec un MongoDB.

````shell script
    > show dbs # Affiche la listes des BDDs
    > use nomBDD # Choisir une BDD
    > db # Affiche la BDD dans laquel on se trouve
    > show collections # Affiche les collections de la BDD  
````
> Il est possible d'utiliser une interface graphique avec l'application : ``MongoDB Compass``
> Il est aussi possible d'héberger sa base de données mongoDB sur le cloud : https://www.mongodb.com/cloud/atlas


---
#### Création d'une BDD
> La commande `create database` n'existe pas en mogonDB
> il faut utiliser la commande :
````shell script
    > use <database_name>
````
> Si la BDD n'existe pas elle sera crée sinon on switchera sur BDD existante
>***Attention*** : Quand la bdd est vide elle n'apparait pas avec un `show dbs`, mais si on fait un `db` on se retrouve bien dans la bdd crée!
---
#### Création d'une collection de manière explicite
````shell script
    > db.createCollection("newCollection1")
      { "ok" : 1 }
````
---

#### Création d'une collection de manière implicite
````shell script
    > db.newCollection2.insert({name : "XXX"})
````
> Si la collection `newCollection2` n'existe pas elle sera crée !
---

````shell script
    > db.newCollection.find()
    > db.newCollection.find().pretty()
    > db.newCollection.findOne()

    
````

---
- ``db.collection.find``
````shell script
db.collection.find( { field1: <value>, field2: <value> ... } )
````

