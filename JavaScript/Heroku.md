# Heroku

Toutes les applications Heroku s'exécutent dans des conteneurs Linux légers appelés dynos qui exécutent la commande spécifiée dans le fichier Procfile.
Par défaut, notre application est déployée sur un dyno gratuit. Les dynos gratuits se mettront en veille après une demi-heure d’inactivité.
Heroku vous permet uniquement de déployer des projets backend ou full-stack. Vous ne pouvez pas utiliser Heroku pour déployer des projets frontaux uniquement sans créer un petit serveur Web pour le servir.

---

- Il faut commencer par installer Command Line Interface (CLI) d'Heroku.
Puis :

````shell script
    cd C:\Program Files\heroku\client\bin
    heroku login # suivre les instructions
````

````shell script
    # se placer a la racine de l'application
    heroku create # création d'une nouvelle application vide sur Heroku, avec un référentiel Git vide associé
      # heroku create
      # Creating app... done, ⬢ pacific-springs-96725
      # https://pacific-springs-96725.herokuapp.com/ | https://git.heroku.com/pacific-springs-96725.git
    git remote -v
      # heroku  https://git.heroku.com/pacific-springs-96725.git (fetch)
      # heroku  https://git.heroku.com/pacific-springs-96725.git (push)
````

> Pour déployer du code sur le dépot distant d'Heroku il faut le faire depuis la branche ``master`` ou ``main``,
> déployer du code depuis une autre branche n'a aucun effet !

````shell script
    git push heroku master
    # Lorsque Heroku reçoit le code source, il récupere toutes les dépendances dans package.json.
````

- création d'un fichier ``Procfile`` sans extension et contenant

````shell script
    web: node app.js # commande qui doit être exécutée pour démarrer notre application.
````

puis :

````shell script
    git add .
    git commit -m "Procfile"
    git push heroku master
````

- Il faut écouter sur le bon port :

````javascript
    // dans la documentation d'heroku :
    let port = process.env.PORT;
    if (port == null || port == "") {
        port = 8000;
    }
    app.listen(port);
    // si la variable d'environnement PORT n'existe pas le server écoutera sur le port 8000

    // Dans mon application :
    let port = process.env.PORT;
    if (port == null || port == "") {
        port = 8000;
    }
    http.listen(port, () => {
        const date = new Date();
        console.log(`${ date.getHours() }H${ date.getMinutes() } on port : ${ port }`);
    });

    // on peut faire aussi directement :
    const PORT = process.env.PORT || 8000;
````

- il faut rajouter un script ``start`` dans le ``package.json``

````json
    {
      "scripts": {
        "start": "node app.js"
      }
    }
````

- il faut rajouter un ``engines`` dans le ``package.json`` avec notre version de node

````json
    "engines": {
      "node": "12.16.1"
    }
````

- s'assurer qu'une instance de notre application est en cours d'exécution :

````shell script
    heroku ps:scale web=1
    heroku ps # affichera le nombre de dinos en cours d'exécution
````

---

````shell script
    heroku open # raccourci pour ouvrir l'application
    heroku local web # démarrer l'application localement
    heroku local # démarrer l'application localement aussi
    heroku apps:info # informations sur l'application notament la taille de l'app
    heroku logs --tail # affiche les logs, très utile pour les erreurs !

    heroku apps:rename newname # renommer une app sur heroku
    #Renaming oldname to newname... done
    #http://newname.herokuapp.com/ | git@herokuapp.com:newname.git
    #Git remote heroku updated
````

---

Ajouter des variables d'environnements dans heroku.

go to  => Settings/Config var (key/value)
