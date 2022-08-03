# Authentification

Il ne faut jamais stocker les mots de passe dans la base de données sans utiliser des algorithmes de `hash`, tel que `MD5` ou `SHA256` afin de transformer une chaine de caractère en une autre. La chaine de caractère `hasher` produira toujours le même résultat.

Un simple `hash` ne suffit pas car il existe des dictionnaires pour retrouver les `hashs` fréquemment utilisés. Pour bloquer les dictionnaires il faut ajouter un texte aléatoire après chaque mot de passe. Le texte aléatoire est appelé le `salt`

## `signUp`

Exemple avec 3 champs : username, email et password

- On génère un `salt` puis

- On génère un `hash` avec la concatenation du `password` + `salt`, ensuite

- On génère un `token` qui est une chaine de caractère aléatoire indépendante qui servira à authentifier un utilisateur grace aux cookies ou localstorage.

- On sauvegarde dans la base de donnée le `username`, `email`, `salt`, `hash`, `token`

## `login`

Exemple ou l'utilsateur se log avec l'`email` et un `password`. Quand l'utilisateur clique sur `se connecter` une requête par au serveur puis le seveur effectue une requête vers la base de données. 

- On recherche l'utilisateur associé à l'email dans la BDD.

- On génère un `hash` avec le `salt` de la BDD et le `password` fourni par l'utilisateur.

- On comparer le `hash` généré et le `hash de la BDD`, s'ils sont pareils c'est le bon password.

- Le serveur renvoie le `token` de l'utilisateur.

- On enregistre le `token` dans les `cookies` ou le  `localstrorage`

## Package côté `backend`

- `uid2` permet de générer des chaînes de caractères aléatoirement en fonction d'une longueur donnée.

- `crypto-js` est une librairie d'algorithmes cryptographiques.

## Package côté `frontend`

- `uuid`

- `bcryptjs`

- `nanoid`
