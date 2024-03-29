# Git

git possède 3 états en local :

Zone 1 : Working directory | Zone 2 : Staging Area | Zone 3 : .git directory
--- | --- | ---
*Untracted or not staged* | *Index* | *Local Repository*
`git add` | `git commit` | `git push`

---

## Modifier la branche principal en master

````shell script
git config –global init.defaultBranch main
````

---

### Vérifier nos paramètres

````shell script
  git config --list
  git config user.name
  git help config 

  # après avoir installé git sur notre machine :
  git config --global user.name "Votre nom ou pseudo"
  git config --global user.email "Votre@email.com"   
````

---

### Initialisation du dépot local

````shell script
    git init # Git crée un répertoire .git qui contient tout ce que Git a besoin pour fonctionner
````

### Lier le dépôt local au dépôt distant (github)

```shell script
    git remote add origin https://github.com/thpGitHub/NonDossier.git
```

### Ajouter les modifications local

````shell script
    git add . # ajoute tous les fichiers présents
    git add *.md
    git add test_*.md
````

````shell script
    git add [nomFichier] # ajoute un fichier   
 ````

````shell script
    git add [nomFichier1] [nomFichier2] # ajoute deux fichiers...
 ````

 ````shell script
    git add . -u # u pour update. Prend uniquement les fichiers déjà suivis et pas les fichiers Untracked
    git add . --dry-run # Simulation pour voir ce que Git va ajouter a l'index
 ````

---

### commit

````shell script
    git commit # va ouvrir un editeur afin de saisir un commentaire
    git commit -v # ajoute le résultat d'un git diff comme commentaire
    git commit -m "Add a file" # ajouter un commentaire en ligne de commande
    git commit -a -m "update file" # permet de passer l'étape de mise à l'index (-a => git add)
    git commit -am "update file" # raccourci de la commande au dessus
````

### On pousse tous les commits vers le dépôt distant

````shell script
    git push origin master
````

---

### Git clone vs git fork

> La principale différence entre le git clone et le fork réside dans le degré de contrôle et d'indépendance que vous souhaitez sur la base de code une fois que vous l'avez copiée.

> Lorsqu'un dépôt Git est cloné, le dépôt cible reste partagé entre tous.

>une opération de fork créera une toute nouvelle copie du référentiel cible. Le développeur qui effectue le fork aura un contrôle total sur la base de code nouvellement copiée.

> Un fork permet d'utiliser des Pull request

### Cloner un dépot existant (vide ou non)

````shell script
    git clone [repo-url]
     # exemple : 
    git clone https://github.com/thpGitHub/Memento.git
    git clone https://github.com/thpGitHub/Memento.git changerNomRepertoire
````

>git clone regroupe plusieurs commandes :
> -Elle crée un nouveau dossier,
> -puis va à l’intérieur de celui-ci et lance git init pour en faire un dépôt Git vide,
> -puis ajoute un serveur distant (git remote add) à l’URL que vous lui avez passée (appelé par défaut origin),
> -puis lance git fetch à partir de ce dépôt distant et
> -ensuite extrait le dernier commit dans votre répertoire de travail avec git checkout.

```shell script
# Après un git clone
git branch
* master
# Les autres branches se cachent
git branch -a
* master
  remotes/origin/HEAD
  remotes/origin/master
  remotes/origin/v1.0-stable
  remotes/origin/experimental
# Pour simplement jeter un coup d'œil rapide sur une branche
git checkout origin/experimental
#Pour travailler sur cette branche
git checkout experimental

git branch
* experimental
  master
```

### Fork un dépot existant

> Il n'est pas possible de fork un repo qui nous appartient !.
Astuce: créer un autre compte github (Avoir quel est l'interet ?) sinon si on veut juste une copie d'un de nos repos on peut dans github importer un repo existant (voir en dessous)

- <https://grafikart.fr/tutoriels/fork-pull-request-591>

Un fork va créer une copie (sur notre github) il faudra faire ensuite un git clone pour l'utiliser en local.
Attention ne jamis travailler sur la branche master mais créer des branches, c'est depuis ces branches

### Importer un dépot existant

- <https://docs.github.com/en/github/importing-your-projects-to-github/importing-source-code-to-github/importing-a-repository-with-github-importer>

---

### git status et git diff

 ````shell script
    git status
    git diff # affichera plus de détail qu'un git status sur les fichiers non indexés. On peut voir les différences des fichiers
    git diff --staged # compare les différences des fichiers indexés
    git diff --cached # même commande que git diff --staged
    git difftool # visualiser les différences avec un outil graphique ou externe
````

### Effacer/supprimer des fichiers

supprimer

````shell script
    rm [nomFichier] # supprimera le fichier (de git et de notre ordi !!) avant l'index (avant un add)
    git rm -f [nomFichier] # supprimera le fichier après l'index (après un add) -f pour forcer la suppression !!
````

effacer

````shell script
    #pour effacer un fichier  il faut l'indexer (apres un add)
    git add [nomFichier]
    git rm --cached [nomFichier] #  cela va annuler le suivi de version du fichier tout en le conservant sur l'ordi
````

### Renommer un fichier

````shell script
    git mv [nom1] [nom2]
````

 ---

### Travailler avec des dépots distants

 <https://git-scm.com/book/fr/v2/Les-bases-de-Git-Travailler-avec-des-d%C3%A9p%C3%B4ts-distants>

 ````shell script
    git remote # affichera le nom cours du/des dépot/s distant/s
    git remote -v # affichera l'url du/des dépot/s distant/s
    git remote add [remote-name] [repo-url] # ajouter un nouveau dépôt distant Git
    git remote show origin # visualiser plus d’informations à propos d’un dépôt distant particulier (ici origin)
    # une info intéressante se trouve à la dernière ligne :
      # (fast-forwardable) ici fast-forwardable signifie qu'il y a des commits qui n'ont pas été push
      # (up to date) ici up to date signifie que la branch du dépot local et la branch du dépot distant sont à jour
      # (local out of date) ici local out of date signifie que le dépot distant a changé par rapport au local
    git remote rename origin toto # renommera le nom du dépot distant origin en toto
    git remote rm toto # retirera le dépot distant

    git fetch [remote-name] # récupère toutes les données que l'on ne possède pas dans notre dépôt local mais sous
    # sa propre branche, ne fusionne pas automatiquement avec nos travaux ni ne modifie notre copie de travail.
    # On doit volontairement fusionner les modifications distantes dans notre travail lorsque nous le souhaitons.
    # Si vous souhaitez fusionner ces données pour que votre branche soit à jour, vous devez utiliser ensuite la commande git merge.
    # Si rien n'est récupéré, rien n'est affiché dans le terminal
    git fetch -v [remote-name] # affichera plus de détails     

    git pull [remote-name] [branch]
    git pull origin master #  récupère et fusionne automatiquement une branche distante (ici master) dans notre branche locale.
    # git pull regroupe les commandes git fetch suivie de git merge. 

    git revert HEAD^ # annulera de dernier commit sur le dépot distant public en créant un nouveau commit
    # ATTENTION : Git revert peut écraser nos fichiers dans notre répertoire de travail, il faudra commiter nos modifications ou de les remiser.
    # git reset  pour faire la même chose, mais sur une branche privée.

    git push origin master # push la branche master du dépot local vers le dépot distant origin
    git push -u origin main # Voir à la fin de ce fichier --set-upstream
    git push origin --all # push toutes les branches locales sur le dépot distant origin
    git push origin --tags # push tous nos tags locaux vers le dépôt distant.
    git push -f heroku main # force le push, écrase le contenu de la branche distante par le contenu de la branch locale

    heroku apps:rename newname # renommer une app sur heroku
    #Renaming oldname to newname... done
    #http://newname.herokuapp.com/ | git@herokuapp.com:newname.git
    #Git remote heroku updated
````

---

### Les branches

- <https://git-scm.com/book/fr/v2/Les-branches-avec-Git-Branches-et-fusions%C2%A0%3A-les-bases>
- <https://nvie.com/posts/a-successful-git-branching-model/#the-main-branches>

````shell script
    git branch -a # affichera les branches du dépot local et celles du dépot distant
    git branch -M main # renommera la branch principale (en général master) en main
    git branch -m new-name # si on est sur la branche que l'on renomme
    git branch -m old-name new-name # si on est sur une branche différente
    git branch testing # création de la branche testing
    git push origin :old-name new-name # supprimer l'ancienne brance distante et push la nouvelle
    git checkout testing # basculer sur la branche testing (potition du pointeur HEAD sur testing)
    # Attention : Git ne vous laissera pas changer de branche si la branche sur laquelle vous êtes n'est pas
    # propre (si il reste des commits à faire par ex.). Mais on peut contourner ceci par le remisage et l’amendement de commit ;)
      git branch -b testing # raccourci pour créér une branche et se potionner dessus
    git checkout -b myfeature develop # création d'une nouvelle branche myfeature depuis develop et on se positionne dessus   
    git log --oneline --decorate
    git log --oneline --decorate --graph --all # va afficher l’historique de vos commits, affichant les endroits où sont 
    # positionnés vos pointeurs de branche ainsi que la manière dont votre historique a divergé.
    git branch # affichera la liste de toutes les branches (* devant une branche: c'est que le pointeur HEAD se situe sur cette branche)
    git branch -v # affichera la liste des derniers commits sur chaque branche
    git branch -vv # affichera la liste des derniers commits sur chaque branche
    # et entre [] les branches en amont (upstream) liées à nos branches locales
    git branch -avv
    git branch -r # même chose que 'git remote show origin' avec moins d'informations
    git branch --merged # affichera quelles branches ont déjà été fusionnées dans votre branche courante (*)
    git branch --no-merged # affichera les branches qui ne sont pas fusionnées (merge)
    git diff branch1..branch2 # affichera la différence entre les deux branches
    git log branch1..branch2 # compare les commits entre deux branches
    git log branch1...branch2 # compare les commits entre deux branches depuis leur ancêtre commun
    git diff branch1..branch2 -- [file] # compare un fichier spécifique
   
````

fusionner des branches

````shell script
    git checkout master # on se potionne sur la branche master
    git merge correctif # on fusionne la branche correctif dans la branche master (création d'un commit de merge)
    # si la mention fast-forward lors du merge apparait c'est qu'il y avait un seul commit d'écart entre la branche master
    # et la branche correctif. GIT va simplement déplacer le pointer master au niveau de correctif.
    git branch -d correctif # suppression de la branche correctif qui ne sert plus à rien car elle a été fusionnée (merge)
    # Attention si on essai de supprimer une branche qui n'a pas été fusionnée (merge) => error: The branch 'correctif' is not fully merged.
    git branch -D correctif # forcer la suppression de la branche correctif
    git push origin --delete [nom_de_la_branche] # supprime la branche sur le dépot distant origin
````

rebaser des branches

> Ne rebasez jamais des commits qui ont déjà été poussés sur un dépôt public.

````shell script

````

---

### Visualiser l'historique des validations

````shell script
    git log # affiche la liste des commits
    git log -p -2 # -p montre les différences et -2 limite à 2
    git log --stat # visualiser des statistiques résumées pour chaque commit
    git log --pretty=oneline #  affiche chaque commit sur une seule ligne
    git log --oneline --decorate --graph --all # ajoute un joli graphe en caractères ASCII pour décrire l’historique des branches et fusions 
    git log --graph --online --first-parent develop # historique seulement de la branche develop
    git log --pretty=format:"%h %s" --graph

    git gitk # interface graphique du dépôt local.

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

    git commit --amend -m "nouveau message de commit" # modifie le message du dernier commit puis push :
    git push --force-with-lease repository-name branch-name

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

---

### Tags

---

### Générer une clé SSH pour l'authentification

````shell script
    ssh-keygen -t rsa -b 4096 -C "example@example.com" # commande à exécuter dans le terminal
    # ensuite aller dans le dossier .ssh dans C:\Users\VotreNomD'Utilisateur\
    # dans ce dossier il y a deux fichiers id_rsa.txt est votre clé privée alors que la clé id_rsa.pub est votre clé publique.
    # il reste a enregistrer cette clé dans les settings de notre compte github
````

---

### Création d'un fichier ``.gitignore``

> Pour demander à git de ne pas suivre certain type de fichiers, il faut créer un fichier ``.gitignore`` à la racine du projet.
> Templates pour ``.gitignore`` : https://github.com/github/gitignore :)

````shell script
    temp # ignore le fichier ou le dossier temp
    /test/temp # ignore le dossier /test/temp
    *.mp3 # ignore tous les fichiers mp3
    test* #ignore tous fichiers commençant par test
    !testfinal #ignore tous fichiers commençant par test sauf testfinal
    .gitignore # on peut aussi ignorer le .gitignore ;), mais il est préférable de ne pas l'ignorer et de le push sur le dépot distant
    # commentaire
````

````shell script
    git rm --cached FILENAME # ignorer un fichier déjà archivé
    git status --ignored # affichera la liste des fichiers ignorés
````

A appronfondir ``.git/info/exclude``

---

### Création d'un fichier ``.gitattributes``

> On peut dans un fichier ``.gitattributes`` (à la racine du projet) gérer les ``merge`` :
 >> Vous pouvez utiliser les attributs Git pour indiquer à Git d'utiliser différentes stratégies de fusion pour des fichiers spécifiques de votre projet. Une option très utile est de dire à Git de ne pas essayer de fusionner des fichiers spécifiques quand ils ont des ***conflits***, mais plutôt d'utiliser votre côté de la fusion par rapport à quelqu'un d'autre.
 >>
 >> Cela est utile si une branche de votre projet a divergé ou est spécialisée, mais que vous souhaitez pouvoir fusionner les modifications à partir de celle-ci et que vous souhaitez ignorer certains fichiers. Supposons que vous ayez un fichier de paramètres de base de données appelé database.xml qui est différent dans deux branches, et que vous souhaitez fusionner dans votre autre branche sans gâcher le fichier de base de données. Vous pouvez configurer un attribut dans le fichier .gitattributes comme celui-ci:
 >>
 ````gitignore
    database.xml merge=ours
````

Et puis définissez une ours stratégie de fusion factice avec:

````shell script
    git config --global merge.ours.driver true
````  
  
 > Si vous fusionnez dans l'autre branche, au lieu d'avoir des conflits de fusion avec le fichier database.xml, vous voyez quelque chose comme ceci:

 ````shell script
    $ git merge topic
      # Auto-merging database.xml
      # Merge made by recursive.
      # Dans ce cas, database.xml reste à la version que vous aviez à l'origine.
````

***ATTENTION*** : s'il n'y a pas de conflit entre les fichiers, le fichier sera mergé dans la branche (ex.: si le fichier de la branche est vide il n'y aura pas de conflit!)

templates pour ``.gitattributes`` : https://gitattributes.io/ ou https://github.com/alexkaratarakis/gitattributes

---

### Définitions

- HEAD :  est une référence sur notre position actuelle dans notre répertoire de travail Git.

### A voir

- <https://devconnected.com/how-to-set-upstream-branch-on-git/>

---

````shell script
#La différence entre
git push origin <branch>
#et
git push --set-upstream origin <branch>
#est qu'ils poussent tous les deux très bien vers le référentiel distant, mais c'est lorsque vous tirez que vous remarquez la différence.

#Si vous faites:
git push origin <branch>
#lorsque vous tirez, vous devez faire:
git pull origin <branch>

#Mais si vous faites:
git push --set-upstream origin <branch>
#alors, lorsque vous tirez, vous n'avez qu'à faire:
git pull

#Ainsi, l'ajout de --set-upstreampermet de ne pas avoir à spécifier de quelle branche vous voulez extraire à chaque fois que vous le faites git pull.
````
