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
- un fichier javascript pour le serveur
- un fichier client HTML
