React
-

Création d'une app React sans ``create react app`` ni de ``CDN``

- création d'un fichier ``package.json`` à la racine du projet
````JSON
    {
  "name": "react-pokemons-app",
  "version": "1.0.0",
  "description": "An awesome application to handle some pokemons.",
  "dependencies": {
    "@types/node": "12.11.1",
    "@types/react": "16.9.9",
    "@types/react-dom": "16.9.2",
    "@types/react-router-dom": "^5.1.2",
    "react": "^17.0.0",
    "react-dom": "^17.0.0",
    "react-router-dom": "^5.1.2",
    "react-scripts": "^4.0.0",
    "typescript": "3.6.4"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build"
  },
  "eslintConfig": {
    "extends": "react-app"
  },
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  }
}
````
- création d'un fichier ``tsconfig.json`` à la racine du projet. Fichier de configuration typescript
````JSON
{
  "compilerOptions": {
    "target": "es5",
    "lib": [
      "dom",
      "dom.iterable",
      "esnext"
    ],
    "allowJs": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react",
    "noFallthroughCasesInSwitch": true
  },
  "include": [
    "src"
  ]
}
````
- ``$ npm install`` pour installer toutes les dépendances du fichier ``package.json`` et va créer un dosiier ``node_modules``
- création d'un fichier ``src`` avec un fichier ``App.tsx`` et ``index.tsx`` qui fera le lien entre ``App.tsx`` et ``index.html``
````
import React from 'react';
  
const App: React.FC = () => {
 const name: String = 'React';
    
 return (
  <h1>Hello, {name} !</h1>
 )
}
  
export default App;
````
````
import React from 'react';
import ReactDom from 'react-dom';
import App from './App';

ReactDom.render(
   <App/>,
   document.querySelector('#root')
);
````
- création d'un dossier ``public`` avec un fichier ``index.
````HTML
<!DOCTYPE html>
<html lang="fr">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Pokédex</title>
  </head>
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root">L'application est en cours de chargement...</div>
  </body>
</html>
````
- lancer l'application ``$ npm start``

---

Il existe deux types de composant dans REACT: 

Composant de fonction                       | Composant de classe
--- | --- 
*Stateless component*                       | *Statefull component*
Plus performant                             |  Moins performant
Plus concis                                 | Plus long à écrire
Pas de gestion du state                     | Gestion du state
Pas de gestion du cycle de vie du composant | Gestion du cycle de vie du composant

- Les composants de fonctions
````typescript
  import React, { FunctionComponent } from 'react';
  
  const App: FunctionComponent = () => {
    const name: String = 'React';
    
    return (
     <h1>Hello, {name} !</h1>
    )
  }
  
  export default App;
````


- Les componsant de classe
````typescript
  import React from 'react';
  
  export default class App extends React.Component {
    const name: String = 'React';
    
    render() {
      return <h1>Hello, {name} !</h1>;
    }
  }
````
---

Les Hooks :

> Les Hooks permettent de rajouter un state à un component et se nomme ``useState``.
Les Hooks sont des fonctions javaScript.
````typescript
  import React, { FunctionComponent, useState } from 'react';
  
const App: FunctionComponent = () => {
 const [name, setName] = useState ('React');
    
 return (
  <h1>Hello, {name} !</h1>
 )
}
  
export default App;

/*
const [name, setName] = useState ('React'); 
===
let nameStateVariable = useState('React');
let name = nameStateVariable[0];
let setName = nameStateVariable[1];
*/

````
