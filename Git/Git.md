# Git
> ### Vérifier nos paramètres
````shell script
  git config --list
  git config user.name
  git help config    
````
> ### Initialisation du dépot local
````shell script
    git init
````
> ### Lier le dépôt local au dépôt distant (github)
```shell script
    git remote add origin https://github.com/thpGitHub/NonDossier.git
```

> ### Ajouter les modifications local
````shell script
    git add . // ajoute tous les fichiers présents
    git add *.md
````

````shell script
    git add nomFichier // ajoute un fichier   
 ````
 
````shell script
    git add nomFichier1 nomFichier2 // ajoute deux fichiers...
 ````

> ### Pour ignorer les modifications dans le workspace (annulation avant de faire un add)
````shell script
    git checkout -- NomDuFichier
````

> ### Pour ignorer les modifications après un add et remettre le fichier dans le workspace
````shell script
    git reset HEAD NomDuFichier
````
> ### commit
````shell script
    git commit # va ouvrir un editeur afin de saisir un commentaire
    git commit -v # ajoute le résultat d'un git diff comme commentaire
    git commit -m "Add a file" # ajouter un commentaire en ligne de commande
    git commit -a -m "update file" # permet de passer l'étape de mise à l'index (-a => git add)
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
     # exemple : 
    git clone https://github.com/thpGitHub/Memento.git
    git clone https://github.com/thpGitHub/Memento.git changerNomRepertoire
````
>   git clone regroupe plusieurs commandes :
> - Elle crée un nouveau dossier,
> - puis va à l’intérieur de celui-ci et lance git init pour en faire un dépôt Git vide,
> - puis ajoute un serveur distant (git remote add) à l’URL que vous lui avez passée (appelé par défaut origin),
> - puis lance git fetch à partir de ce dépôt distant et
> - ensuite extrait le dernier commit dans votre répertoire de travail avec git checkout.
 
 > ### git status et git diff
 ````shell script
    git status
    git diff # affichera plus de détail qu'un git status sur les fichier pas encore indexé. On peut voir les différence des fichiers
    git diff --staged #compare les différence des fichiers indexés
    git difftool # visualiser les différences avec un outil graphique ou externe
````

> ### Effacer/supprimer des fichiers

supprimer
````shell script
    rm nomFichier # supprimera le fichier (de git et notre ordi !!) avant l'index (avant un add)
    git rm -f nomFichier # supprimera le fichier après l'index (après un add) -f pour forcer la suppression !!
````
effacer
````shell script
    #pour effacer un fichier  il faut l'indexé (apres un add)
    git add nomFichier
    git rm --cached nomFichier #  cela va annuler le suivi de version du fichier tout en le conservant sur l'ordi
````

> ### Renomer un fichier
````shell script
    git mv nom1 nom2
````

 ---
 
 ### Travailler avec des dépots distants
 https://git-scm.com/book/fr/v2/Les-bases-de-Git-Travailler-avec-des-d%C3%A9p%C3%B4ts-distants
 ````shell script
    git remote # affichera le nom cours du/des dépot/s distant/s
    git remote -v # affichera url du/des dépot/s distant/s
    git remote add [nomcourt] [url] # ajouter un nouveau dépôt distant Git
    git remote show origin # visualiser plus d’informations à propos d’un dépôt distant particulier (ici origin)
    git remote rename origin toto # renomera le nom du dépot distatnt origin en toto
    git remote rm toto # retirera le dépot distant
````
---
### Les branches
https://git-scm.com/book/fr/v2/Les-branches-avec-Git-Branches-et-fusions%C2%A0%3A-les-bases
````shell script
    git branch testing # création de la branche testing
    git checkout testing # basculer sur la branche testing (potition du pointeur HEAD sur testing)
    # Attention : Git ne vous laissera pas changer de branche si la branche sur laquelle vous êtes n'est pas
    # propre (si il reste des commits à faire par ex.). Mais on peut contourner ceci par le remisage et l’amendement de commit ;)
      git branch -b testing # raccourci pour créér une branche et se potionner dessus
    git log --oneline --decorate
    git log --oneline --decorate --graph --all # va afficher l’historique de vos commits, affichant les endroits où sont 
    # positionnés vos pointeurs de branche ainsi que la manière dont votre historique a divergé.
    git branch # affichera la liste de toutes les branches (* devant une branche: c'est que le pointeur HEAD se situe sur cette branche)
    git branch -v # affichera la liste des derniers commits sur chaque branche
    git branch --merged # affichera quelles branches ont déjà été fusionnées dans votre branche courante (*)
    git branch --no-merged # affichera les branches qui ne sont pas fusionnées (merge)
   
````
fusionner des branches
````shell script
    git checkout master # on se potionne sur la branche master
    git merge correctif # on fusionne la branche correctif dans la branche master
    # si la mention fast-forward lors du merge apparait c'est qu'il y avait un seul commit d'écart entre la branche master
    # et la branche correctif. GIT va simplement déplacer le pointer master au niveau de correctif.
    git branch -d correctif # suppression de la branche correctif qui ne sert plus à rien car elle a été fusionnée (merge)
    # Attention si on essai de supprimer une branche qui n'a pas été fusionnée (merge) => error: The branch 'correctif' is not fully merged.
    git branch -D correctif # forcer la suppression de la branche correctif
````

---
### Visualiser l'historique des validations
````shell script
    git log # affiche la liste des commits
    git log -p -2 # -p montre les différences et -2 limite a 2
    git log --stat # visualiser des statistiques résumées pour chaque commit
    git log --pretty=oneline #  affiche chaque commit sur une seule ligne
    git log --oneline --decorate --graph --all # ajoute un joli graphe en caractères ASCII pour décrire l’historique des branches et fusions 
    git log --pretty=format:"%h %s" --graph
````
---

### Annuler des actions
````shell script

````

---

### Création d'un fichier ``.gitignore``
> Pour demander à git de ne pas suivre certain type de fichiers, il faut créer un fichier ``.gitignore à la racine du projet``
>***Attention*** les fichiers déjà suivis par git ne pourront pas être ignorés
templates pour ``.gitignore`` : https://github.com/github/gitignore :)
````shell script
    # ignore le fichier ou le dossier temp
    temp 
    # ignore le dossier /test/temp
    /test/temp
    # ignore tous les fichiers mp3
    *.mp3
    #ignore tous fichiers commençant par test sauf testfinal
    test* 
    !testfinal
    # on peut aussi ignorer le .gitignore ;)
    .gitignore
````