Heroku
-

Il faut commencer par installer Command Line Interface (CLI) d'Heroku.
Puis :


````shell script
    cd C:\Program Files\heroku\client\bin
    heroku login # suivre les instructions
````
````shell script
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
````

création d'un fichier ``Procfile`` sans extension et contenant
````text
    web: node app.js
````
puis :
````shell script
    git add .
    git commit -m "Procfile"
    git push heroku master

````