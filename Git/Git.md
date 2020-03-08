# Git
> #### Initialisation du dépot local
````shell script
    git init
````
> #### Lier le dépôt local au dépôt distant (github)
```shell script
    git remote add origin https://github.com/thpGitHub/NonDossier.git
```
> #### Pour ignorer les modifications dans le workspace (annulation avant de faire un add)
````shell script
    git checkout -- NomDuFichier
````
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
> #### Pour ignorer les modifications après un add et remettre le fichier dans le workspace
````shell script
    git reset HEAD NomDuFichier
````
> #### commit
````shell script
    git commit -m"Add a file"
````
> #### On pousse tous les commits vers le dépôt distant
````shell script
    git push origin master
````