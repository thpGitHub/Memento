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
    git add . # ajoute tous les fichiers présents
    git add *.md
````

````shell script
    git add nomFichier # ajoute un fichier   
 ````
 
````shell script
    git add nomFichier1 nomFichier2 # ajoute deux fichiers...
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

> ### Cloner un dépot existant (vide ou non)

````shell script
    git clone [repo-url]
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
    git remote add [remote-name] [repo-url] # ajouter un nouveau dépôt distant Git
    git remote show origin # visualiser plus d’informations à propos d’un dépôt distant particulier (ici origin)
    git remote rename origin toto # renomera le nom du dépot distatnt origin en toto
    git remote rm toto # retirera le dépot distant

    git fetch [remote-name] # récupère toutes les données que l'on ne possède pas dans notre dépôt local mais sous
    # sa propre branche, elle ne les fusionne pas automatiquement avec nos travaux ni ne modifie notre copie de travail.
    # On doit volontairement fusionner les modifications distantes dans notre travail lorsque nous le souhaitons.
    # Si vous souhaitez fusionner ces données pour que votre branche soit à jour, vous devez utiliser ensuite la commande git merge.

    git pull [remote-name] [branch]
    git pull origine master #  récupère et fusionne automatiquement une branche distante (ici master) dans notre branche locale.
    # git pull regroupe les commandes git fetch suivie de git merge. 

    git remote show [remote-name]
    git remote rename [remote-name] [new_remote_name] #renomer un nom de dépot distant

    git remote rm [remote-name] # retirer un dépot distant

    git revert HEAD^ # annulera de dernier commit sur le dépot distant public en créant un nouveau commit
    # ATTENTION : Git revert peut écraser nos fichiers dans notre répertoire de travail, il faudra commiter nos modifications ou de les remiser.
    # git reset  pour faire la même chose, mais sur une branche privée. 
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
    git reflog # affiche l'historique des commits ainsi que toutes les autres actions faites en local

    git checkout 93eaf21 # se potitionnner sur un commit

    git blame monFichier.js # va afficher pour chaque ligne modifiée, son ID, l'auteur, l'horodatage, le numéro de la ligne et le contenu de la ligne

    git cherry-pick d356940 de966d4 # va dupliquer les commits d356940 de966d4 dans notre branche
````
---

### Annuler des actions
````shell script
    git commit -m 'validation initiale' # on fait un commit puis on s'apperçois d'un oublie
    git add fichier_oublie # on rajoute a l'index le fichier oublié
    git commit --amend # puis on commit. ammend modifie le dernier commit
    git commit --amend --no-edit # permet de modifier le commit sans changer le message

    git commit --amend -m "nouveau message de commit" # modifie le message du dernier commit

    git reset HEAD fichier # désindexera le fichier

    git reset --hard HEAD^ # supprime de la branche courante le dernier commit. Le Head^ indique que c'est bien le dernier commit que nous voulons supprimer.
    # ATTENTION : si on veux récupérer le commit supprimer il faut avant récupérer son hash (git log)
    git branch -b testing # nous pouvons créér une nouvelle branche afin de récupérer le dernier commit supprimé
    git reset --hard [sha_commit] # seul les 8 premier caractère du hash du commit sont nécessaire.    

    git checkout -- fichier # annulera toutes les modifications faites dans le fichier. Attention : les modifications seront perdues !

    git stash # placera en remise le ou les fichiers présents dans l'index et l'enlèvera de la branche courante
    git stash list # liste des remises avec leur identifiant
    git branch -b testing # nous pouvons créér une nouvelle branche pour y ajouter notre remise
    git stash apply [identifiant-remise] # stash@{0}.....

    git reset # Si rien n'est spécifié après git reset, par défaut il exécutera un git reset --mixed HEAD~
    git reset --soft # permet juste de se placer sur un commit spécifique afin de voir le code à un instant donné ou créer une branche
    # partant d'un ancien commit. Il ne supprime aucun fichier, aucun commit, et ne crée pas de HEAD détaché.
    git reset --mixed # revenir juste après votre dernier commit ou le commit spécifié, sans supprimer vos modifications en cours.
    # Il va créer un HEAD détaché.
    # Il permet aussi, dans le cas de fichiers indexés mais pas encore commités, de désindexer les fichiers.    
    git reset [Commit-Cible] --hard # revenir au commit cible en supprimant DEFINITIVEMENT tous les commits suivants



````

### Générer une clé SSH pour l'authentification
````shell script
    ssh-keygen -t rsa -b 4096 -C "example@example.com" # commande à exécuter dans le terminal
    # ensuite aller dans le dossier .ssh dans C:\Users\VotreNomD'Utilisateur\
    # dans ce dossier il y a deux fichiers id_rsa.txt est votre clé privée alors que la clé id_rsa.pub est votre clé publique.
    # il reste a enregistrer cette clé dans les settings de notre compte github
````

---

### Création d'un fichier ``.gitignore``
> Pour demander à git de ne pas suivre certain type de fichiers, il faut créer un fichier ``.gitignore`` à la racine du projet
>***Attention*** les fichiers déjà suivis par git ne pourront pas être ignorés
templates pour ``.gitignore`` : https://github.com/github/gitignore :)

````shell script
    temp # ignore le fichier ou le dossier temp
    /test/temp # ignore le dossier /test/temp
    *.mp3 # ignore tous les fichiers mp3
    test* #ignore tous fichiers commençant par test
    !testfinal #ignore tous fichiers commençant par test sauf testfinal
    .gitignore # on peut aussi ignorer le .gitignore ;)
````

### Définitions :

 - HEAD :  est une référence sur notre position actuelle dans notre répertoire de travail Git. 

