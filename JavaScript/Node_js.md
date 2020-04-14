Node.js
-
> Node est le runtime (environnement d'exécution, un runtime ne permet que l'exécution d'un programme. il peut être vu comme une machine virtuelle) qui permet d'écrire toutes nos tâches côté serveur, en JavaScript.
> Node ajoute également des fonctionnalités que le JavaScript du navigateur standard ne possède pas,
> comme par exemple l'accès au système de fichiers local.

initialiser un projet node 
````shell script
    npm init
````
il y  a des options par defaults à valider ou à modifier, ses options seront enregistrées dans un fichier ``package.json``:
On peut modifier par exemple l'option : ``entry point: (index.js)`` qui a par default le fichier index.js

### lancement dans un terminal d'un fichier :
````shell script
    node nomDuFichier.js
````


 
   Logiciels serveur :
 -
   - Un logiciel serveur est démarré en permanence sur une machine :
     -> On appelle ce type de programmmes DAEMON (sur Linux/MacOS) ou SERVICE (sur Windows)
   - Un logiciel serveur est identifié par un numéro compris entre 1 et 65535 :
     -> le numéro de PORT logiciel
   - Un logiciel comprends des messages formaté selon les règles d'un PROTOCOLE
     -> pour le Web le protocole s'appelle HTTP
   - Le rôle d'un logiciel serveur est :
     -> de recevoir des messages : des requêtes généralement
     -> d'envoyer des réponses.
   - Pour le Web, Un logiciel serveur "télécharge" (DOWNLOAD) des requêtes et "téléverse" (UPLOAD) des réponses.
   - Pour le Web, Un navigateur Internet "téléverse" (UPLOAD) des requêtes et "télécharge" (DOWNLOAD) des réponses.
   
  
   Dans le cas du HTTP, on peut créer des serveurs HTTP qui sont spécialisé dans l'envoi de code(S) qui peuvent :
   - S'afficher dans un navigateur Internet (code pdf, code jpeg, code HTML, ...);
   - S'exécuter dans un navigateur Internet (JavaScript).
   
   ````javascript
    // création de l'objet http à partir du module http de Node JS    
    const http = require('http');
    // création de l'objet server à partir de la méthode creatServer() du module http
    const server = http.createServer(function(req, res) {
      res.writeHead(200);
      res.end('Salut tout le monde !');
    });
    server.listen(8080);
````
   