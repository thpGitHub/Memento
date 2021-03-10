# WEBPACK

> **attention** voir plus bas une technique plus moderne d'utiliser webpack.

Le principe de webpack est simple, on lui donne un ou plusieurs fichiers à l'entrée (entry) et on a un fichier en sortie (output).

Le but de Webpack est de parcourir les fichiers, de trouver les relations entre les fichiers (import, export, require) et de fournir des solutions pour packager tous les fichiers qu’il trouve ensemble. Lorsqu’il identifie des fichiers JS, il sait les manipuler, les concaténer. S’il rencontre des éléments qui ne sont pas du JavaScript, alors il a besoin de loader pour savoir les interpréter.

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

Un seul fichier `app.bundle.js` dans le dossier `dist` a été généré (alors qu'il y avait deux fichiers js). Webpack s'est fié au import et export pour générer un seul fichier.

---

## Transpiler avec Babel

Transpiler pour rendre notre code JavaScript compatible avec les navigateurs les moins récents !
Dans nos fichiers d'exemple nous utilisons async  et  await  qui sont apparus dans une version récente de JavaScript que tous les navigateurs ne supportent pas encore.

````shell script
npm install --save-dev babel-loader @babel/core @babel/preset-env babel-polyfill
````

Il faut dans le fichier `webpack.config.js` de webpack ajouter Babel dans les `rules`, les `rules` sont des règles de webpack indiquant les loaders à utiliser sur quels fichiers.

````javascript
const path = require('path');

module.exports = {
  mode: "production",
  entry: {
    polyfill: "babel-polyfill",
    app: "./src/index.js"
  },
  output: {
    filename: "[name].bundle.js",
    path: path.resolve(__dirname, "dist")
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader",
          options: {
            presets: ["@babel/preset-env"]
          }
        }
      }
    ]
  }
};
````

Babel sera exécuté sur tous les fichiers JavaScript du projet, sauf ceux qui se trouvent dans le dossier `node_modules`.
Dans `entry` on a rajouté `polyfill: "babel-polyfill"` cela veut dire que lorsque nous compilons notre code, deux fichiers vont être générés.
Il faut mettre a jour le fichier HTML.

````HTML
<body>
        <script src="./dist/polyfill.bundle.js"></script>
        <script src="./dist/app.bundle.js"></script>
</body>
````

---

## Webpack-dev-server

Est un serveur pour tester votre code et qui recharge automatiquement votre navigateur dès que nous modifions notre code.

````shell script
npm install --save-dev webpack-dev-server
````

````json
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack",
    "start-server": "webpack serve --open 'Chrome'"
  }
````

``ATTENTION`` il se peut que webpack considère que les fichiers sont générés à la racine du server et non dans le dossier `dist`.

````HTML
<body>
        <script src="/polyfill.bundle.js"></script>
        <script src="/app.bundle.js"></script>
</body>
````

````shell script
npm run start-server
# Project is running at http://localhost:8080/
````

"presets": ["@babel/preset-env", "@babel/preset-react"]

---

## Technique plus moderne d'utiliser webpack

````shell script
mkdir webpack-tutorial
cd webpack-tutorial
npm init -y
npm i webpack webpack-cli webpack-dev-server --save-dev
````

Dans `package.json`

````json
"scripts": {
    "dev": "webpack --mode development"
  },
````

Le point d'entrée par default de webpack est `src/index.js`

````shell script
mkdir src
echo 'console.log("Hello webpack!")' > src/index.js

npm run dev
# Cela va créer un nouveau dossier `dist` avec un fichier bundle Webpack `main.js`
````

Configuration de Webpack

````shell script
touch webpack.config.js
````

Dans ce fichier il faut au minimun un `module.exports` qui est l'exportation de common JS pour Node.js.
Exemple du fichier `webpack.config.js` pour changer le point d'entrée et la sortie.

````javascript
const path = require("path");

module.exports = {
  entry: { index: path.resolve(__dirname, "source", "index.js") }
};
// webpack recherchera `source/index.js` comme fichier d'entré
````

````javascript
const path = require("path");

module.exports = {
  output: {
    path: path.resolve(__dirname, "build")
  }
};
// webpack placera le bundle fichier dans le répertoire `build` à la place de `dist`
````

Installation du plugin `html-webpack-plugin` pour travailler avec HTML dans webpack. Le pluging chargera nos fichier HTML et il *injecte directement* le(s) bundle(s) dans le même fichier (fini l'import des scripts `.../dist/polyfill.bundle.js">` `.../dist/app.bundle.js` dans le fichier HTML)

````shell script
npm i html-webpack-plugin --save-dev
````

````javascript
const HtmlWebpackPlugin = require("html-webpack-plugin");
const path = require("path");

module.exports = {
  plugins: [
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, "src", "index.html")
    })
  ]
};
````

Utilisation du serveur de développement webpack.Ce package a été installé au début (`npm i webpwebpack-cli webpack-dev-server --save-dev`)

Dans le `package.json` ajouter le script `start`

````json
"scripts": {
    "dev": "webpack --mode development",
    "start": "webpack serve --open 'Firefox'",
  },
````

````shell script
npm start
````

### Loaders webpack

Webpack ne comprend que les fichiers JavaScript et JSON.Les loaders permettent à webpack de traiter d'autres types de fichiers et de les convertir en modules valides qui peuvent être consommés par votre application et ajoutés au graphe de dépendances.

- Loaders css

````javascript
// charger un fichier css dans index.js
import "./style.css";
console.log("Hello webpack!");
````

installation des 2 loaders pour le css : `css-loader` et `style-loader`

````shell script
npm i css-loader style-loader --save-dev
````

````javascript
//webpack.config.js
const HtmlWebpackPlugin = require("html-webpack-plugin");
const path = require("path");

module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader"]
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, "src", "index.html")
    })
  ]
};
````

````shell script
npm start
# le feuille de style devrait être chargée dans le head du bundler html (dist/index.html)
````

**A voir** : MiniCssExtractPlugin : <https://webpack.js.org/plugins/mini-css-extract-plugin/>

**Attention** L'ordre des loaders est importante, les loaders dans webpack sont chargés de droite à gauche ( `css-loader` en premier puis `style-loader`)

- Loaders SASS

Pour travailler avec SASS dans webpack nous devons installer 3 loarders (2 loaders css et un loader sass)

````javascript
// charger un fichier css dans index.js
import "./style.scss";
console.log("Hello webpack!");
````

````shell script
npm i css-loader style-loader sass-loader sass --save-dev
# or only
npm i sass-loader sass --save-dev
````

````javascript
//webpack.config.js
const HtmlWebpackPlugin = require("html-webpack-plugin");
const path = require("path");

module.exports = {
  module: {
    rules: [
      {
        test: /\.scss$/,
        use: ["style-loader", "css-loader", "sass-loader"]
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, "src", "index.html")
    })
  ]
};
````

- Loaders JavaScript moderne avec babel

Babel est un compilateur et un tranpilateur. Babel est capable de transformer en code compatible pour n'importe quel navigateur.
Pour travailler avec babel dans webpack nous devons installer 3 loarders.

````shell script
npm i @babel/core babel-loader @babel/preset-env --save-dev
````

````javascript
const HtmlWebpackPlugin = require("html-webpack-plugin");
const path = require("path");

module.exports = {
  module: {
    rules: [
      {
        test: /\.scss$/,
        use: ["style-loader", "css-loader", "sass-loader"]
      },
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader",
          options: {
            presets: ["@babel/preset-env"]
          }
        }
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, "src", "index.html")
    })
  ]
};
````

````shell script
npm run dev
# Le code transformé est dans `dist/main.js`
````

Autre solution : création d'un fichier de config `babel.config.json`

````json
{
  "presets": [
    "@babel/preset-env"
  ]
}
````

````javascript
const HtmlWebpackPlugin = require("html-webpack-plugin");
const path = require("path");

module.exports = {
  module: {
    rules: [
      {
        test: /\.scss$/,
        use: ["style-loader", "css-loader", "sass-loader"]
      },
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: ["babel-loader"]
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, "src", "index.html")
    })
  ]
};
````

### Configuration de `React` `Webpack` et `Babel` à partir de zéro

Pour utiliser React avec webpack en parallèle au loader babel on doit installer le préréglage (preset) babel pour React `@babel/preset-react`.

````shell script
npm i @babel/core babel-loader @babel/preset-env @babel/preset-react --save-dev
````

````javascript
// webpack.config.js
const HtmlWebpackPlugin = require("html-webpack-plugin");
const path = require("path");

module.exports = {
  module: {
    rules: [
      {
        test: /\.scss$/,
        use: ["style-loader", "css-loader", "sass-loader"]
      },
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader",
          options: {
            presets: ["@babel/preset-env", "@babel/preset-react"]
          }
        }
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, "src", "index.html")
    })
  ]
};
````

````shell script
npm i react react-dom
````

````javascript
// src/index.js
import React, { useState } from "react";
import { render } from "react-dom";

function App() {
    const [state, setState] = useState("CLICK ME");

    return <button onClick={() => setState("CLICKED")}>{state}</button>;
}

render(<App />, document.getElementById("root"));
````

````html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Webpack tutorial</title>
    </head>
    <body>
        <h1>Hello Thierry sur Webpack !</h1>
        <p>Hello sass!</p>
        <div id="root"></div>
    </body>
</html>
````

````shell script
npm start
````

### Mode production

Webpack a deux modes de fonctionnement: le `development` et `production`.

- En mode `development` aucune minification n'est appliquée.
- En mode `production` une minification est faite avec `TerserWebpackPlugin` et un `scope hoisting` avec `ModuleConcatenationPlugin`
- La variable d'environnement `process.env.NODE_ENV` est mise à `production`

````json
"scripts": {
    "dev": "webpack --mode development",
    "start": "webpack serve --open 'Firefox'",
    "build": "webpack --mode production"
  },
````

````shell script
npm run build
````

---

## ANNEXES

## Il y a deux manières de lancer webpack 5

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
