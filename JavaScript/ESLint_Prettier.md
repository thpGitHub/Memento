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

Afin de voir directement les erreurs dans notre éditeur (VS Code pour moi) on doit aussi installer l'extension `EsLint` dans VS Code.

Si on utilise pas `Create React App` il faudra installer ESLint comme indiquer dans la doc : <https://eslint.org/docs/user-guide/getting-started>

## Prettier

Il est important d' installer Prettier localement dans chaque projet, afin que chaque projet reçoive la bonne version de Prettier.

<https://prettier.io/docs/en/install.html>

Contrairement à `Eslint`, `Prettier` n'est pas installé de base avec `Create React App`.

````shell script
npm install --save-dev --save-exact prettier
````

puis création de deux fichiers à la racine du projet :

`.prettierrc.json`et `.prettierignore`

````javascript
// .prettierrc exemple
{
  "arrowParens": "avoid",
  "bracketSpacing": false,
  "endOfLine": "lf",
  "htmlWhitespaceSensitivity": "css",
  "insertPragma": false,
  "jsxBracketSameLine": false,
  "jsxSingleQuote": false,
  "printWidth": 80,
  "proseWrap": "always",
  "quoteProps": "as-needed",
  "requirePragma": false,
  "semi": false,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "all",
  "useTabs": false
}
````

````javascript
// .prettierignore exemple 
node_modules
coverage
build
````

````shell script
npx prettier --write . # on peut formater tous les fichiers avec Prettier
npx prettier --write app/ # formater que le dossier app
npx prettier --write app/components/Button.js
npx prettier --check . # vérifie seulement que les fichiers sont déjà formatés, plutôt que de les écraser
# npx nous permet d'exécuter des outils installés localement !
````

---

> Si on souhaite activer et désactiver le formatage de `Prettier` on peut installer dans VS Code l'extension : `Formatting Toggle`
> On pourra cliquer en bas à droite dans la barre de tache sur `Formatting`

## ESLint avec Prettier

Surtout utile si on modifie `ESLint` avec des règles qui rentrent en conflit avec les règles de `Prettier` ou que l'on importe une config `ESLint` populaire comme `eslint-config-airbnb` <https://www.npmjs.com/package/eslint-config-airbnb> déjà configuré.

Pour éviter tout conflit entre `ESLint` et `Prettier` <https://github.com/prettier/eslint-config-prettier#installation>

````shell script
npm install --save-dev eslint-config-prettier
# pour désactive toutes les règles inutiles ou susceptibles d'entrer en conflit avec Prettier .
````

## Husky
