# L'API Fetch

> interface JavaScript pour l'accès et la manipulation des parties de la pipeline HTTP, comme les requêtes et les réponses. Cela fournit aussi une méthode globale ``fetch()`` qui procure un moyen facile et logique de récupérer des ressources à travers le réseau de manière asynchrone.
> Ce genre de fonctionnalité était auparavant réalisé avec XMLHttpRequest.

<https://www.pierre-giraud.com/javascript-apprendre-coder-cours/api-fetch/>

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
fetch('http://localhost:3001/pokemons')
   .then(response => response.json())
   .then((data) => {
      console.log(data);
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

>bien que la méthode soit nommée json(), le résultat n'est pas JSON mais est plutôt le résultat de la prise en entrée de JSON et de son analyse pour produire un objet JavaScript.

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

## Fournir des options à la requête

La méthode fetch() accepte un second paramètre, optionnel ; un objet init qui vous permet de contrôler un certain nombre de réglages :

- <https://developer.mozilla.org/fr/docs/Web/API/Fetch_API/Using_Fetch#fournir_des_options_%C3%A0_la_requ%C3%AAte>

````javascript
var myHeaders = new Headers();

var myInit = { method: 'GET',
               headers: myHeaders,
               mode: 'cors',
               cache: 'default' };

fetch('flowers.jpg',myInit)
.then(function(response) {
  return response.blob();
})
.then(function(myBlob) {
  var objectURL = URL.createObjectURL(myBlob);
  myImage.src = objectURL;
});
````

A voir pour le header de fetch :

- <https://github.github.io/fetch/>

- URLSearchParams : application/x-www-form-urlencoded;charset=UTF-8
- FormData : multipart/form-data
