# Git
> #### Initialisation du dépot local
````shell script
    git init
````
> #### Lier le dépôt local au dépôt distant (github)
```shell script
    git remote add origin https://github.com/thpGitHub/NonDossier.git
```
> #### Ajouter les modifications local
````shell script
    git add . // ajoute tous les fichiers présents
````

````shell script
     git add nomFichier // ajoute un fichier   
 ````
 
````shell script
     git add nomFichier1 nomFichier2 // ajoute deux fichiers...
 ````
> #### commit
````shell script
    git commit -m"Add a file"
````
> #### On pousse tous les commits vers le dépôt distant
````shell script
    git push origin master
````