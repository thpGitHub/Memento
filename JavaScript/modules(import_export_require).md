# Les modules JavaScript

- <https://v8.dev/features/modules#mjs>

Un module est un un fichier JavaScript qui va exporter du code placé dans un fichier séparé.

On va ensuite pouvoir choisir quels éléments du module vont être exportés ``export``, afin de les importer :

- dans le navigateur web avec ``import`` (module ECMAScript)
- en dehors du navigateur Web (dans NodeJS) avec ``require`` (module CommonJS). ``require`` peut également être utilisé côté navigateur, mais le code doit être empaqueté avec un transpilateur car les navigateurs ne supportent pas CommonJS

dans d’autres modules ou dans d’autres scripts.

---

## exemple avec un simple fichier js (NodeJS)

`server.js`

````javascript
require('./database_connect');
````

`database_connect.js`

````javascript
const mongoose = require('mongoose');
require('dotenv').config();

const connection = process.env.DB_HOST_ATLAS;
// const connection = process.env.DB_HOST_LOCAL;

mongoose.connect(connection,{ useNewUrlParser: true,
                              useUnifiedTopology: true,
                              useFindAndModify: false})
    .then(() => console.log("Database Connected Successfully"))
    .catch(err => console.log(err));
````

## exemple avec une classe (NodeJS)

`server.js`

````javascript
const DatabaseConnection = require('./database_connection');

DatabaseConnection.connect();
````

`database_connection.js`

````javascript
const mongoose = require('mongoose');
require('dotenv').config();

class DatabaseConnection {
    static connection = process.env.DB_HOST_ATLAS;

    static connect () {
        mongoose.connect(this.connection,{ useNewUrlParser: true,
            useUnifiedTopology: true,
            useFindAndModify: false})
            .then(() => console.log("Database Connected Successfully"))
            .catch(err => console.log(err));
    }
}

module.exports = DatabaseConnection;
````

---

- https://www.pierre-giraud.com/javascript-apprendre-coder-cours/module-import-export/#:~:text=En%20r%C3%A9sum%C3%A9,pr%C3%A9c%C3%A9dant%20d'une%20d%C3%A9claration%20export%20.
- https://stackoverflow.com/questions/9901082/what-is-this-javascript-require
- https://stackoverflow.com/questions/46677752/the-difference-between-requirex-and-import-x
- https://masteringjs.io/tutorials/node/import-vs-require
- https://www.grafikart.fr/tutoriels/javascript-modules-1354

- https://v8.dev/features/modules#mjs

- https://www.digitalocean.com/community/tutorials/understanding-modules-and-import-and-export-statements-in-javascript


