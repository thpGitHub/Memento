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
> #### git pull ou git fetch ?
> Les deux commandes mettent à jour un répertoire de travail local avec les données d'un repository distant
````shell script
    git fetch
````
> git fetch va récupérer toutes les données des commits effectués sur la branche courante qui n'existent pas encore dans votre version en local.
> Ces données seront stockées dans le répertoire de travail local mais ne seront pas fusionnées avec votre branche locale.
> Si vous souhaitez fusionner ces données pour que votre branche soit à jour, vous devez utiliser ensuite la commande git merge.
````shell script
    git pull
````
> git pull regroupe les commandes git fetch suivie de git merge.

λ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes not staged for commit:
(use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   syntaxe/browse_Object_and_Array.md
    deleted:    syntaxe/function_declared_vs_function_expression.md

    no changes added to commit (use "git add" and/or "git commit -a")


    j'ai fait git add -A ( cela inclura les fichiers supprimés.)
    
    ---
    Pour supprimer des fichiers de l'étape, utilisez reset HEAD où HEAD est le dernier commit de la branche courante. Cela décompactera le fichier mais conservera les modifications.
    
    git reset HEAD <file>