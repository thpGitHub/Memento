# L'API Fetch

> interface JavaScript pour l'accès et la manipulation des parties de la pipeline HTTP, comme les requêtes et les réponses. Cela fournit aussi une méthode globale ``fetch()`` qui procure un moyen facile et logique de récupérer des ressources à travers le réseau de manière asynchrone.
> Ce genre de fonctionnalité était auparavant réalisé avec XMLHttpRequest. 

````javascript
    fetch(url)
      .then((res) => {
        // handle response
      })
      .catch((error) => {
        // handle error
      });
````

````javascript
    // https://jsonplaceholder.typicode.com/users
    fetch('https://jsonplaceholder.typicode.com/users')
        .then(function(response) {
            return response.json();
        })
        .then(function(data) {
            console.log(data)
        });
````

Il y a plusieurs typage de données pour la ``response`` : 

- response.arrayBuffer()
- response.blob()
- response.json()
- response.text()
- response.formData()

Attrapper les erreurs :

````javascript
    // https://jsonplaceholder.typicode.com/users
    fetch('https://jsonplaceholder.typicode.com/users')
        .then(function(response) {
            return response.json();
        })
        .then(function(data) {
            console.log(data)
        })
        .catch(function(err) {
            console.error(err);
        });
````

---
Si on souhaite utiliser Fetch avec node et non dans le navigateur :

````shell script
    npm i node-fetch
````

````javascript
    const fetch = require("node-fetch");
````
