# ESLint, Prettier et Husky

`ESLint` analyse statiquement notre code pour trouver les problèmes.

`Prettier` va formater notre code en suivant des règles prédéfinies.

## ESLint

Si on utilise `Create React App` pour créer notre projet `Eslint` est automatiquement installé. Nous pouvons le voir dans le fichier `package.json` :

````json
"eslintConfig": {
    "extends": [
      "react-app",
      "react-app/jest"
    ]
  },
````

Afin de voir directement les erreurs dans notre éditeur (VS Code pour moi) on doit aussi installer l'extension `EsLint`

Si on utilise pas `Create React App` il faudra installer ESLint comme indiquer dans la doc : <https://eslint.org/docs/user-guide/getting-started>

## Prettier

Contrairement à `Eslint`, `Prettier` n'est pas installé de base avec `Create React App`.



## Husky
