# Git
> ### Initialisation du dépot local
````shell script
    git init
````
> ### Lier le dépôt local au dépôt distant (github)
```shell script
    git remote add origin https://github.com/thpGitHub/NonDossier.git
```
> ### Pour ignorer les modifications dans le workspace (annulation avant de faire un add)
````shell script
    git checkout -- NomDuFichier
````
> ### Ajouter les modifications local
````shell script
    git add . // ajoute tous les fichiers présents
````

````shell script
    git add nomFichier // ajoute un fichier   
 ````
 
````shell script
    git add nomFichier1 nomFichier2 // ajoute deux fichiers...
 ````
> ### Pour ignorer les modifications après un add et remettre le fichier dans le workspace
````shell script
    git reset HEAD NomDuFichier
````
> ### commit
````shell script
    git commit -m"Add a file"
````
> ### On pousse tous les commits vers le dépôt distant
````shell script
    git push origin master
````
> ### git pull ou git fetch ?
> Les deux commandes mettent à jour un répertoire de travail local avec les données d'un repository distant
````shell script
    git fetch
````
> git fetch va récupérer toutes les données des commits effectués sur la branche courante qui n'existent pas encore dans votre version en local.
> Ces données seront stockées dans le répertoire de travail local mais ne seront pas fusionnées avec votre branche locale.
> Si vous souhaitez fusionner ces données pour que votre branche soit à jour, vous devez utiliser ensuite la commande git merge.   
> git pull regroupe les commandes git fetch suivie de git merge.
````shell script
    git pull
````
>  ! [rejected]        master -> master (non-fast-forward)   
> hint: Updates were rejected because the tip of your current branch is behind   
  hint: its remote counterpart. Integrate the remote changes (e.g.   
  hint: 'git pull ...') before pushing again.   
````shell script
    git pull <remote> <branch>
    git pull origine master
````

> ### Cloner un dépot existant (vide ou non)

````shell script
    git clone <repo-url>
     // exemple : 
    git clone https://github.com/thpGitHub/Memento.git
````
>   git clone regroupe plusieurs commandes :
> - Elle crée un nouveau dossier,
> - puis va à l’intérieur de celui-ci et lance git init pour en faire un dépôt Git vide,
> - puis ajoute un serveur distant (git remote add) à l’URL que vous lui avez passée (appelé par défaut origin),
> - puis lance git fetch à partir de ce dépôt distant et
> - ensuite extrait le dernier commit dans votre répertoire de travail avec git checkout.
