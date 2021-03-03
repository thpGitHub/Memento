# WEBPACK

Le principe de webpack est simple, on lui donne un ou plusieurs fichiers à l'entrée (entry) et on a un fichier en sortie (output).

Le but de Webpack est de parcourir les fichiers, de trouver les relations entre les fichiers et de fournir des solutions pour packager tous les fichiers qu’il trouve ensemble. Lorsqu’il identifie des fichiers JS, il sait les manipuler, les concaténer. S’il rencontre des éléments qui ne sont pas du JavaScript, alors il a besoin de loader pour savoir les interpréter.

Babel est un exemple de loader bien connu.

De base, Webpack ne sait que manipuler du JS mais il inclut aussi des plugins de minification. Ainsi quand on lance Webpack en mode production, il va automatiquement faire des optimisations avec de la minification.

La version simple de Webpack vient avec son système de fonctionnement qui inclut un certain nombre de loaders et de plugins de base. Le loader de base lit du JS et sait interpréter les require dans un JS en CommonJS. En revanche, il ne sait pas lire du HTML ou du CSS. C’est pour cette raison qu’on a besoin de loaders dédiés au CSS et au HTML, et de Babel pour faire tout ce qui est ES6.

````shell script
    npm install webpack webpack-cli --save-dev
````

Création d'un fichier de configuration : ``webpack.config.js`` à la racine du projet.

````javascript
const path = require('path');

module.exports = {
  mode: "production",
  entry: {
    app: "./src/index.js"
  },
  output: {
    filename: "[name].bundle.js",
    path: path.resolve(__dirname, "dist")
  }
};
````

> Webpack va se servir du fichier `./src/index.js` comme point d'entrée et bundler notre code dans `./dist/app.bundle.js` (`[name]` correspond au name du fichier `package.json`).

Dans le fichier ``index.html`` :

````html
    <body>
        <script src="./dist/bundle.js"></script>
    </body>
````

Création de deux fichiers .js pour l'exemple

index.js

````javascript
import retrieveContent from './query.js';

async function showContent() {
  try {
    const content = await retrieveContent();

    let elt = document.createElement('div');
    elt.innerHTML = content.join('<br />');

    document.getElementsByTagName('body')[0].appendChild(elt);
  } catch (e) {
    console.log('Error', e);
  }
}

showContent();
````

query.js

````javascript

export default async function retrieveContent() {
  const url = "https://baconipsum.com/api/?type=all-meat&paras=2&start-with-lorem=1";

  const response = await fetch(url);
  return response.json();
}

````

Modification du fichier `package.json` pour ajouter un script

````json
"scripts": {
    "test": "...",
    "build": "webpack"
}
````

Nous pouvons maintenat compiler notre projet

````shell script
npm run build
````

Un seul fichier `app.bundle.js` dans le dossier `dist` a été généré (alors qu'il y avait deux fichier js). Webpack s'est fié au import et export pour générer un seul fichier.

---

## Transpiler avec Babel

---

## Webpack-dev-server

---

## Il y a deux manières de lancer webpack

- Dans le terminal :

````shell script
    $ node_modules\.bin\webpack
    # exécution de webpack se trouvant dans le dossier node_modules...
    # du projet
````

Si on veux que webpack se relance automatiquement :

````shell script
    node_modules\.bin\webpack --watch
````

- Ajouter un script dans le fichier `package.json` du projet :

Nous allons créer deux scripts ``build`` et ``dev`` pour lancer les deux commandes du terminal ci-dessus :

````javascript
    "scripts": {
        "build": "webpack",
        "dev": "webpack --watch"
      },
````

Executer les scripts dans le terminal :

````shell script
    npm run build
    npm run dev
````
