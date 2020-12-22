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

# Il existe deux types de composant dans REACT: 

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

# Les Hooks :

> Les Hooks permettent de rajouter un ``state`` à un component et se nomme ``useState``.
Les Hooks sont des fonctions javaScript.
````typescript
  import React, { FunctionComponent, useState } from 'react';
  
const App: FunctionComponent = () => {
 const [name, setName] = useState<string> ('React');
    
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
> Les hooks permettent de rajouter des méthodes de  ``cycle de vie`` pour les composants et se nomme ``useEffect`` :

- ``componentDidMount() ``
C'est la méthode appelée en premier lors de la création d'un composant lorsqu'il est inséré dans le DOM.
Cela permet de mettre en place certaines instructions lors de l'initialisation du composant, comme la récupération de données depuis un serveur distant par exemple.
On parle de ``montage`` du composant.

- ``componentDidUpdate(prevProps, prevState)``
Dès que les valeurs d'une propriété du composant sont modifiées, le composant est mis à jour.
La méthode reçoit en paramètre deux objets représentant les ``props`` et le ``state`` avant la mise à jour.
Cela permet de travailler sur le DOM une fois que le composant a été mis à jour.

- ``componentWillUnmount()``
C'est la dernière méthode de cycle de vie d'un composant, appelé juste avant que le composant soit détruit par React.
Un composant est détruit lorsqu'il est retirer du DOM. Cette méthode permet de se désabonner de certaines dépendances du composant et ainsi éviter les problèmes de performance.
Cette étape est appelé ``démontage``.

````typescript
    import React, { FunctionComponent, useState, useEffect } from 'react';
    import Pokemon from './models/pokemon';
    import POKEMONS from './models/mock-pokemon';
  
    const App: FunctionComponent = () => {
      const [pokemons, setPokemons] = useState<Pokemon[]> ([]);
   
      useEffect( () => {
        setPokemons(POKEMONS);
      }, []);

    return (
      <h2>Il y a  {pokemons.length} pokemons !</h2>
    )
    };
  
export default App;
````

### 3 règles à respecter avec les Hooks
- On ne doit pas appeler les hooks à l'intérieur d'une boucle ou d'une condition. Il faut utiliser les hooks à la racine du composant fonction.
- On appel les hooks uniquement depuis des composants de fonction ! Exception : on peut appeler un hook depuis un autre composant que l'on a créé (hooks personnalisé).
- Quand on utilise la méthode du hook (ex. set name...) la valeur est remplacée complètement et non fusionnée !

# JSX et le DOM virtuel 

- Gestionnaire d'événement REACT :
````typescript
import React, { FunctionComponent, useState, useEffect } from 'react';
import Pokemon from './models/pokemon';
import POKEMONS from './models/mock-pokemon';
  
const App: FunctionComponent = () => {
     const [pokemons, setPokemons] = useState<Pokemon[]> ([]);
   
     useEffect( () => {
          setPokemons(POKEMONS);
     }, []);

     // cette variable est un gestionnaire d'événement.
     const showPokemonsCount = () => {
          console.log(pokemons.length);
     };

     return (  // ici commence le JSX
          <div>
               <h1>Pokédex</h1>
               // les évènements REACT sont en camel case par rapport aux évents JS.
               <p onClick={showPokemonsCount}>Afficher le nombre de Pokémons</p>
          </div>
     )
};
  
export default App;
````
- Gestionnaire d'événement natif JavaScript
````typescript
import React, { FunctionComponent, useState, useEffect } from 'react';
import Pokemon from './models/pokemon';
import POKEMONS from './models/mock-pokemon';
  
const App: FunctionComponent = () => {
     const [pokemons, setPokemons] = useState<Pokemon[]> ([]);
   
     useEffect( () => {
          setPokemons(POKEMONS);
     }, []);

     // cette variable est un gestionnaire d'événement.
     const showPokemonsCount = () => {
          console.log(pokemons.length);
     };

     return (  // ici commence le JSX
          <div>
               <h1>Pokédex</h1>
               // les évènements REACT sont en camel case par rapport aux évents JS.
               <p onClick={showPokemonsCount}>Afficher le nombre de Pokémons</p>
          </div>
     )
};