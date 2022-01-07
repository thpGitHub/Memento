# Fork

## `git clone` vs `fork`

Lorsque l'on `git clone` un dépôt sur github cela va créér une copie du dépot sur notre ordinateur. Ce dépot sera lié au dépot cloné et si vous avez les droits d'écriture vous pourrez envoyer (`push`) vos modifications directement sur le dépot d'origine.
Le `git clone` d'un dépot ne permet pas les `pull request`.

Lorsque  l'on `fork` un dépot sur github cela va créer une copie du dépot dans notre compte github, cette copie pourra être fusionnée avec le dépot d'origine via une `pull request`. Cette `pull request` proposera au propriétaire du dépot de fusionner (`merge`) les modifications dans son dépot.
Le `fork` agit comme un intermédiaire entre l'utilisateur et le propriétaire du dépot.

## `fork` d'un dépot sur github

- aller sur le dépots que l'on souhaite `fork`

- sur github clicker sur l'icone de `fork`

<img width="100"
      alt="fork github"
      src="../assets/fork_github.PNG"
/>

- une copie vient d'être créér sur notre compte github

- cloner cette copie avec `git clone` afin d'avoir une autre copie sur votre ordinateur (dépot local) qui sera liée au dépot distant (action automatique avec la commande git clone).

````shell script
# pour vérifier à quel dépot est lié notre dépot local
git remote -v
````

- installer les dépendances

- sur ce dépot local créer une branche et travaillé dessus.

- envoyer (`push`) cette branche sur la copie du `fork` qui se trouve sur votre compte github.

- aller sur le `fork` qui se trouve sur votre compte github et une icone est apparue :
