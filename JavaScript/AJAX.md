# AJAX (Asynchronous JavaScript And XML)

- [Requête GET](#reqGet)
- [Requête POST](#reqPost)
- [AJAX avec jQuery](#jquery)
- [AJAX avec Axios](#axios)
- [AJAX avec RXJS](#rxjs)
- [AJAX avec Fetch](#fetch)

> Asynchrone signifie que l'application Web peut envoyer et recevoir des données du serveur Web sans actualiser la page.
> AJAX utilise un objet ``XMLHttpRequest`` intégré au navigateur pour demander des données à un serveur Web.
> Tous les navigateurs modernes ont un XMLHttpRequestobjet intégré.
> L'objet ``XMLHttpRequest`` : Est une API sous la forme d'un objet dont les méthodes aident au transfert de données entre un navigateur Web et un serveur Web.
> Les applications AJAX peuvent utiliser XML pour transporter des données, mais il est tout aussi courant de transporter des données en texte brut ou en texte JSON.

Créez une instance de l'objet XMLHttpRequest:

````javascript
    let xhttp = new XMLHttpRequest();
````

Propriétés de l'objet XMLHttpRequest:
(attention il y en a d'autres : responseText, responseXML, statusText)

``readystate`` est une propriété de l'objet XMLHttpRequest qui contient le statut de XMLHttpRequest.

- 0: demande non initialisée
- 1: connexion au serveur établie
- 2: demande reçue
- 3: traitement de la demande
- 4: demande terminée et réponse prête

``onreadystatechange`` est une propriété de l'objet XMLHttpRequest qui définit une fonction à appeler lorsque la propriété readyState change.
``onreadystatechange``  contient le gestionnaire d'évènement appelé lorsque l'évènement readystatechange est déclenché.

``status`` est une propriété de l'objet XMLHttpRequest qui retourne le numéro d'état d'une requête

- 200: "OK"
- 304: "Not Modified" (redirection implicite vers une ressource mise en cache)
- 403: "Interdit"
- 404: "Introuvable"

Méthodes d'objet ``XMLHttpRequest``:

Pour envoyer une demande à un serveur Web, nous utilisons les méthodes ``open()`` et ``send()`` de l'objet ``XMLHttpRequest``.

````javascript
    function changeContent() {
        let xhttp = new XMLHttpRequest();
        // onreadystatechange est un gestionnaire d'évènement (EventHandler) invoqué à chaque fois que l'attribut readyState change.
        xhttp.onreadystatechange = function() {
            if (this.readyState === 4 && this.status === 200) {
             document.getElementById("foo").innerHTML = this.responseText;
             //console.log(xhttp);
            }
        };
        // XMLHttpRequest.open(method, url, async, user, password)
        // le 3eme parametre par default est a true, si false la requete devient synchrone
        xhttp.open("GET", "content.txt", true);
        xhttp.send();
    }
````

> Ici on envoie une requête mais nous ne traitons pas la réponse reçue <a id="reqGet"></a>:

````javascript
    const request = new XMLHttpRequest();
    request.open("GET","https://www.prevision-meteo.ch/services/json/vaucresson");
    request.send();
````

> Avec le traitement de la réponse :

````javascript
    const request = new XMLHttpRequest();
    
    request.onreadystatechange = function () {
      //if (httpRequest.readyState === XMLHttpRequest.DONE) {
        if (this.readyState === 4 && this.status === 200) {
            let reponse = JSON.parse(this.responseText);
            console.log(reponse.current_condition.condition);
            document.body.innerHTML = reponse.current_condition.condition;
        }
    };
    request.open("GET","https://www.prevision-meteo.ch/services/json/vaucresson");
    request.send();
````

## Envoyer des données dans l'url

````javascript
    request.open('get', url+`?pseudo=${ e.target.value }`);
    request.send();
    // autre exemple :
    request.open("GET", "demo_get2.asp?fname=Henry&lname=Ford");
    request.send();
````

---

## Requête POST <a id="reqPost"></a>

Notez que si vous voulez envoyer des données avec la méthode POST, vous aurez peut-être à changer le type MIME de la requête. Par exemple, utilisez ce qui suit avant d’appeler send() pour envoyer des données de formulaire en tant que chaîne de requête :
httpRequest.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');

````javascript
    request.open('post', url);
    request.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
    request.send(`pseudo=${ e.target.value }`);
    // envoyer plusieurs valeurs :
    // request.send("fname=Henry&lname=Ford");
````

Exemple dans du code JS :

````javascript
    const eltPseudo = document.querySelector('#pseudo')
            .addEventListener('keyup', (e) => {
                const request = new XMLHttpRequest();
                const url     = '/test';// route pour le serveur express
    
                // request.open('get', url+`?pseudo=${ e.target.value }`);
                // request.send();
                request.open('post', url);
                request.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
                request.send(`pseudo=${ e.target.value }`);
    
                request.onload = function () {
                    console.log('la reponse = ', request.response);
                    if (request.response === 'ok le pseudo existe dans la bdd') {
                        document.querySelector('#pseudo_keep').innerHTML = `Attention le pseudo ${ e.target.value } est déja pris !`
                    }
                }
            })
````

la partie serveur express :

````javascript
    app.post('/test', (req, res) => {
        console.log(req.body);
    
        MongoClient.connect(url,{useNewUrlParser:true, useUnifiedTopology:true}, (err, client) => {
            if (err) {
                return console.log('err');
            }
    
            const db = client.db(dbName);
            const myCollection = db.collection('login');
            myCollection.find({ pseudo: req.body.pseudo }).toArray((err, docs) => {
                client.close();
                console.log(docs);
                //res.render('accueil', {title: 'Accueil', datas: docs});
                if (docs.length) {
                    res.send('ok le pseudo existe dans la bdd');
                    //res.render('game_arena');
                }else {
                    //res.render('index', { message: 'Pseudo ou Mot de passe incorrect' });
                    res.send('ko le pseudo n existe PAS dans la bdd');
                }
            });
        });
        //console.log(req.query.autrenom);
        //res.render('test');
        //res.send('ok');
    });
````

---

## AJAX avec jQuery <a id="jquery"></a>

> Callback Hell
---

## AJAX avec Axios <a id="axios"></a>

Axios est une bibliothèque JavaScript permettant d'effectuer des requêtes HTTP depuis Node ou XMLHttpRequest ou un navigateur. Il est isomorphe (= il peut s'exécuter dans le navigateur et nodejs avec la même base de code). En tant que bibliothèque moderne, elle est basée sur l'API Promise. Axios présente certains avantages, comme la protection contre les attaques de falsification de requêtes intersites (CSFR). Pour pouvoir utiliser la bibliothèque Axios, les utilisateurs doivent l'installer et l'importer dans votre projet, en utilisant CDN, npm, Yarn ou Bower.

> Fourni des promesses (la promesse renvoie une seule valeur).
> Axios est une bibliothèque JavaScript fonctionnant comme un client HTTP.

***A voir plus tard*** function generator js   ``function*``

````shell script
    $ npm i axios
````

````javascript
    axios({  url: 'https://dog.ceo/api/breeds/list/all',
             method: 'get',
             data: {   
                 foo: 'bar' 
             }
    })
````

On peut utiliser :

- ``axios.get()``
- ``axios.post()``

## Typage TypeScript

Afin d'obtenir les typages TypeScript (pour intellisense / saisie semi-automatique) lors de l'utilisation des importations CommonJS, require()utilisez l'approche suivante :

````javascript
const axios = require('axios').default;
````

## Requête GET

````javascript
const axios = require('axios');

// Make a request for a user with a given ID
axios.get('/user?ID=12345')
  .then(function (response) {
    // handle success
    console.log(response);
  })
  .catch(function (error) {
    // handle error
    console.log(error);
  })
  .then(function () {
    // always executed
  });

// Optionally the request above could also be done as
axios.get('/user', {
    params: {
      ID: 12345
    }
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  })
  .then(function () {
    // always executed
  });  

// Want to use async/await? Add the `async` keyword to your outer function/method.
async function getUser() {
  try {
    const response = await axios.get('/user?ID=12345');
    console.log(response);
  } catch (error) {
    console.error(error);
  }
}
````

## Requete POST

````javascript
    axios.post('/user', {
        firstName: 'Fred',
        lastName: 'Flintstone'
      })
      .then(function (response) {
        console.log(response);
      })
      .catch(function (error) {
        console.log(error);
      });
````

Exécution de plusieurs requêtes simultanées :

````javascript
function getUserAccount() {
  return axios.get('/user/12345');
}

function getUserPermissions() {
  return axios.get('/user/12345/permissions');
}

Promise.all([getUserAccount(), getUserPermissions()])
  .then(function (results) {
    const acct = results[0];
    const perm = results[1];
  });
````

---

## AJAX avec RXJS <a id="rxjs"></a>

> Fourni des observables (un observable renvoie plusieurs valeurs)

````shell script
    $ npm i rxjs
````

````javascript
    // import de la methode ajax() de rxjs
    import { ajax } from "rxjs/ajax";

    const observable = ajax({
       url: "https://pokeapi.co/api/v2/pokemon",
       method: "get", 
    });
    // la méthode ``subscribe()`` déclenche la requête,
    // il y a 3 méthodes : ``next()`` ``error()`` ``complete()``
    observable.subscribe({
        next() {
        // appelée à chaque fois qu'une nouvelle valeur est émise
        },
        error() {
        // appelée quand une erreur se produit
        },
        complete() {
        // appelée quand l'observable a fini d'émettre des valeurs
        }
    });
````

Avec un exemple :

````javascript
    import { ajax } from "rxjs/ajax";
    
    const observable = ajax({
       url: "https://pokeapi.co/api/v2/pokemon",
       method: "get",
    });
    
    const ul = document.querySelector('ul');
    
    observable.subscribe({
        next(response) {
            const result = response.response.results;
    
            for (let pok of result) {
                let li = document.createElement('li');
                li.innerHTML = pok.name;
                ul.appendChild(li);
            }
        },
        error(error) {
            console.log('error', error);
        },
        complete() {
            console.log('complete')
        }
    });

````

---

## AJAX avec Fetch <a id="fetch"></a>

> Disponible dans tous les navigateurs modernes

Annexes :

<https://betterprogramming.pub/why-javascript-developers-should-prefer-axios-over-fetch-294b28a96e2c>
