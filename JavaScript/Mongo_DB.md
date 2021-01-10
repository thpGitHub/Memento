# MongoDB

> Après avoir installé mongoDB (attention pour l'instant je suis dans : C:\Program Files\MongoDB\Server\4.2\bin)
> on peut commencer par vérifier les services windows qui sont sur notre machine
> dans recherche taper simplement ``services`` et le service se nomme : ``mongoDB Server``

````shell script
    # lancer dans le répertoire ou se trouve mongo.exe
    $ mongo
    # On peut éviter de se déplacer dans le répertoire en modifiant la variable système PATH
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
    > db.version() # Connaitre la version de mongo

    $ mongo --version # Connaitre la version de mongo hors interface mongo
````

> Il est possible d'utiliser une interface graphique avec l'application : ``MongoDB Compass``
> Il est aussi possible d'héberger sa base de données mongoDB sur le cloud : https://www.mongodb.com/cloud/atlas

---

## Création d'une BDD

> La commande `create database` n'existe pas en mogonDB
> il faut utiliser la commande :

````shell script
    > use <database_name>
````

> Si la BDD n'existe pas elle sera crée sinon on switchera sur BDD existante
>***Attention*** : Quand la bdd est vide elle n'apparait pas avec un `show dbs`, mais si on fait un `db` on se retrouve bien dans la bdd crée!
---

## Création d'une collection de manière explicite

````shell script
    > db.createCollection("newCollection1")
      { "ok" : 1 }
````

---

## Création d'une collection de manière implicite

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

---

## Importer une BDD dans mongo

- Dans un terminal

````shell script
        # Importer le contenu du fichier contact.json dans une base données users et dans une collection contacts
        $ mongoimport --db users --collection contacts --file contacts.json
        $ mongoimport --db users --collection contacts --file c:\Users\xxxx\Downloads\contacts.json
        $ mongoimport --db users --collection contacts --file /Users/xxxx/Downloads/contacts.json

        $ mongoimport -d users -c contacts < contacts.json
````

- Dans l'interface mongo

````shell script
        > load("C:/Program Files/MongoDB/Server/4.0/data/files/contact.js")
````

---

## CRUD

> Il y a 3 façons d'insérer un document dans mongoDB
>> Insérer un document avec `insert`
>>> Insérer un document avec `update`
>>>> Insérer un document avec `save`

- Create

````script shell
# Syntaxe : db.collection.insert(document)
> db.personne.insert({prenom:"thierry})
> db.personne.insert({_id:4,prenom;"thierry",nom:"po"})

# Syntaxe : db.collection.save(document)
> db.personne.save({_id:4,prenom;"thierry",nom:"po"})
# si _id 4 existe il le remplace sinon il se comporte comme un insert
````

- Read

- UPDATE

````script shell
# Syntaxe : db.collection.update(CRITERIA, UPDATE_DATA, OPTION)
# update remplace le premier document trouvé par un autre document
> db.personne.update({prenom:"toto"},{nom:"titi})

# Syntaxe : db.collection.update(CRITERIA,$ser{UPDATE_DATA}, Option)
# update peut aussi changer (au lieu de remplacer un document par un autre) la valeur d'un(ou +ieurs) champ(s)
> 

````

- DELETE
