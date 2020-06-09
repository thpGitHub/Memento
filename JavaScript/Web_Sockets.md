WebSockets
---
> WebSocket est une API JavaScript qui  permet d'ouvrir un canal de communication bidirectionnelle
> entre un navigateur (côté client) et un serveur.    
> Les WebSockets sont une technologie basée sur le protocole web socket.

- URL en HTTP : http[s]://hôte||ip[:port][/ressource][?query string]
- URL en WS : ws[s]://hôte||ip[:port]   

### socket.io
> socket.io est une bibliothèque pour utiliser les WebSockets plus facilement.
> Un socket est une connection permanante avec un serveur.
````shell script
    $ npm i socket.io
````
> Quand on utilise socket.io on doit toujours s'occuper de deux fichiers en même temps : 
- un fichier javascript pour le serveur qui centralise et gère les connexions des différents clients connectés au site.
- un fichier client HTML qui se connecte au serveur et qui affiche les résultats dans le navigateur.   

Le client effectue donc 2 types de connexion :

-    Une connexion "classique" au serveur en HTTP pour charger la page index.html

-   Une connexion "temps réel" pour ouvrir un tunnel via les WebSockets grâce à socket.io

``index.js``
````javascript
    const app  = require('express') (),
          port = 65000,
          http = require('http').createServer(app),
          io   = require('socket.io')(http);
    
    app.get('/', (req, res) => {
        console.log(__dirname);
       res.sendFile(__dirname + '/index.html');
    });
    // event de type connection
    io.on('connection', (socket) => {
        console.log('a user connected');
    });

    http.listen(port, () => {
        const date = new Date();
        console.log(`${date.getHours()}H${date.getMinutes()}`);
    });
````
``index.html``
````html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>get started Chat</title>
    </head>
    <body>
       
        <script src="/socket.io/socket.io.js"></script>
        <script>
            let socket = io();
        </script>
    </body>
    </html>
````

event sur les sockets : ```disconnect```
````javascript
    io.on('connection', (socket) => {
      console.log('a user connected');
      socket.on('disconnect', () => {
        console.log('user disconnected');
      });
    });
````

> 1/ Emettre des évènements depuis le navigateur vers le serveur : ``socket.emit();``   
Depuis le fichier ``index.html``
````html
    <script>
            var socket = io();
            const send_form = document.querySelector('form').addEventListener('submit', (e)=> {
                e.preventDefault();
                console.log('SEND !');
                socket.emit('chat message', document.querySelector('#m').value);
                document.querySelector('#m').value ='';
    
            })
        </script>
````

> 2/ Reception de l'event depuis le serveur 
Depuis ``ìndex.js``
````javascript
    io.on('connection', (socket) => {
      socket.on('chat message', (msg) => {
        console.log('message: ' + msg);
      });
    });
````

> 3/ Emettre des event depuis le serveur vers les utilisateurs :
Pour envoyer un événement à tout le monde : ``io.emit()``.
Depuis ``index.js``
````javascript
    io.on('connection', (socket) => {
          socket.on('chat message', (msg) => {
          console.log('message: ' + msg);
          io.emit ( 'chat message' , msg); 
          });
        });
````
> 4/ Capturer l'event envoyé par le serveur   
Depuis le ``index.html``
````html
    <script>
            var socket = io();
            const send_form = document.querySelector('form').addEventListener('submit', (e)=> {
                e.preventDefault();
                console.log('SEND !');
                socket.emit('chat message', document.querySelector('#m').value);
                document.querySelector('#m').value ='';
                return false;
            });
            socket.on('chat message', function(msg){
                const li = document.createElement('li');//.innerHTML = msg;
                li.innerHTML = msg;
                document.querySelector('#messages').appendChild(li);
            });
        </script>
````
