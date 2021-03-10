# React

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

- ``$ npm install`` pour installer toutes les dépendances du fichier ``package.json`` et va créer un dossier ``node_modules``
- création d'un dossier ``src`` avec un fichier ``App.tsx`` et ``index.tsx`` qui fera le lien entre ``App.tsx`` et ``index.html``

````typescript
import React from 'react';
  
const App: React.FC = () => {
 const name: String = 'React';
    
 return (
  <h1>Hello, {name} !</h1>
 )
}
  
export default App;
````

````typescript
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

## Il existe deux types de composant dans REACT

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
  
  const App: FunctionComponent = () => { // correspond : const App: React.FC = () => {
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

## Les Hooks

> Les Hooks permettent de rajouter un ``state`` à un composant de fonction et se nomme ``useState``.
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

> Les hooks permettent de rajouter des méthodes de  ``cycle de vie`` à un composants de fonction et se nomme ``useEffect``.

Il y a trois méthodes ``de cycle de vie`` d'un composant dans React :

- `` componentDidMount() ``
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

## 3 règles à respecter avec les Hooks

- On ne doit pas appeler les hooks à l'intérieur d'une boucle ou d'une condition. Il faut utiliser les hooks à la racine du composant fonction.
- On appel les hooks uniquement depuis des composants de fonction ! Exception : on peut appeler un hook depuis un autre composant que l'on a créé (hooks personnalisé).
- Quand on utilise la méthode du hook (ex. setName...) la valeur du state est remplacée complètement et non fusionnée !

---

## JSX et le DOM virtuel

 Gestionnaire d'événement REACT :

``onclick`` devient en react ``onClick``

``mousseenter`` devient en react ``onMouseEnter``

etc...

En CSS `border-color` devient `borderColor`.
L'attribut style en jsx doit être un objet JavaScript

````typescript
<h1 style={{color: "red"}}>Hello Style!</h1>

function App() {
  const style = {
    padding: '40px',
    textAlign: 'center',
  }

  return <div style={style}>Welcome to React!</div>
}

````

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

Récupérer l'évènement natif javaScript :

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
     const addPokemon = (name: string, e: any) => {
          console.log(e);
          if (e.nativeEvent.which === 1) {
               console.log(`Le pokémon ${name} a été ajouté au pokédex, avec le click gauche`);
          } else {
               console.log(`Le pokémon ${name} a été ajouté au pokédex, avec le click droit`)
          }
     };
     return (  
          <div>
               <h1>Pokédex</h1>
               // les évènements REACT sont en camel case par rapport aux évents JS.
               <p onClick={showPokemonsCount}>Afficher le nombre de Pokémons</p>
               <button onClick={(e) => addPokemon('toto', e)}>Ajouter un Pokémon !</button>
          </div>
     )
};
  
export default App;
````

## Les conditions dans JSX

*ATTENTION* JSX comprend les ``expressions`` javascript mais ne comprend pas les ``instructions`` javascript

Astuce ici avec l'operateur &&

````typescript
  return (
    <div>
      {age > 18 && // si la condition est true le paragraphe sera affiché grace à l'oppérateur &&
        <p>Vous êtes majeur, donc vous pouvez voir ce contenu.</p>}
    </div>
  );
````

Astuce ici avec l'operateur ternaire

````typescript
  return (
    <p> {age > 18 ? (
      Vous êtes majeur, donc vous pouvez voir ce contenu.
    ) : (
      Vous êtes mineur, il faut attendre.
    )};
  );
`````

## Afficher une liste de tableau avec la méthode native map() dans JSX

> Lorsqu’on affiche une liste dans du code JSX, il est important d’appliquer la propriété key, sous peine de lever une erreur.

````typescript
  return (
   <ul>
   // attention                V  ici () et non {}
    {pokemons.map((pokemon) => (
      <li key={pokemon.name}>{pokemon.name}</li>
    ))} 
   </ul>
  );
`````

````typescript
  return (
   <ul>
    {pokemons.map((pokemon, index) => (
      <li key={index}>{pokemon.name}</li>
    ))} 
   </ul>
  );
`````

Avec le destructuring on peut récupérer juste le nom :

````typescript
  return (
   <ul>
    {pokemons.map(({name}) => (
      <li key={name}>{name}</li>
    ))} 
   </ul>
  );
`````

---

## Les props

> Pour passer une props à un composant il faut l'ajouter en paramètre de la fonction du composant.
> On passe des props depuis un composant parent

````typescript
return (
  // Ici nous passons la props pokemon au composant PokemonCard
  <PokemonCard pokemon={ pokemon } />
);
````

````typescript

interface Props  {
    pokemon: Pokemon
};

// ici le composant PokemonCard n'acceptera une seul props de type Pokemon
const PokemonCard: FunctionComponent<Props> = ({ pokemon }) => {

    return (
        
      <div>
        {/* <h1>Pokédex</h1> */}
        <div>  
            <div className="ff">
               <div>
                  <img src={ pokemon.picture } alt={ pokemon.name }/>
               </div>
               <div>
                  <div>{ pokemon.name }</div>
                  <div>{ pokemon.created.toString() }</div>
               </div>
            </div>            
        </div>
    </div>

    );
}

export default PokemonCard;
````

Props par defaut grace à typeScript

````typescript
interface Props  {
    pokemon: Pokemon,
    borderColor?: string
};

const PokemonCard: FunctionComponent<Props> = ({ pokemon, borderColor = 'olive' }) => {

    return (
        
      <div>
        {/* <h1>Pokédex</h1> */}
        <div>  
            <div className="ff" style={{ borderColor: borderColor }}>
               <div>
                  <img src={ pokemon.picture } alt={ pokemon.name }/>
               </div>
               <div>
                  <div>{ pokemon.name }</div>
                  <div>{ pokemon.created.toString() }</div>
               </div>
            </div>            
        </div>
    </div>

    );
}

export default PokemonCard;
````

````typescript
return (
  // Ici la props borderColor écrasera la valeur par defaut dans le composant PokemonCard
  <PokemonCard pokemon={ pokemon } borderColor="tomato"/>
);
````

exemple

````typescript
import  React, { FunctionComponent,useState, useEffect } from 'react';
import Pokemon from '../models/pokemon';
import './pokemon-card.css'

interface Props  {
    pokemon: Pokemon,
    borderColor?: string
};

const PokemonCard: FunctionComponent<Props> = ({ pokemon, borderColor = 'olive' }) => {

    const [color, setColor] = useState<string>();

    const showBorder = () => {
        setColor(borderColor);
    }
    const hideBorder = () => {
        setColor("whitesmoke");
    }

    return (
        
      <div>
        {/* <h1>Pokédex</h1> */}
        <div>  
            <div className="ff"
                 style={{ borderColor: color }} 
                 onMouseEnter={ showBorder } 
                 onMouseLeave={ hideBorder }>
               <div>
                  <img src={ pokemon.picture } alt={ pokemon.name }/>
               </div>
               <div>
                  <div>{ pokemon.name }</div>
                  <div>{ pokemon.created.toString() }</div>
               </div>
            </div>            
        </div>
    </div>

    );
}

export default PokemonCard;
````

---

## les propriétée calculée

> Sont des fonctions dans un composent pour transformer des données avant l'affichage dans le DOM. Ici nous allons créér une fonction formatDate().

````typescript
import React, { FunctionComponent, useState } from 'react';
import Pokemon from '../models/pokemon';
import './pokemon-card.css';
  
type Props = {
  pokemon: Pokemon,
  borderColor?: string
};
  
const PokemonCard: FunctionComponent<Props> = ({ pokemon, borderColor = '#009688' }) => {
    
  const [color, setColor] = useState<string>();

  const showBorder = () => {
      setColor(borderColor);
  };  

  const hideBorder = () => {
      setColor('#f5f5f5'); // on remet la bordure en gris.s
  };  

  const formatDate = (date: Date): string => {
      return `${date.getDate()}/${date.getMonth()+1}/${date.getFullYear()}`;
  };

  return (
    <div className="col s6 m4" onMouseEnter={ showBorder } onMouseLeave={ hideBorder }>
      <div className="card horizontal" style={{ borderColor: color }}>
        <div className="card-image"> 
          <img src={ pokemon.picture } alt={ pokemon.name }/>
        </div>
        <div className="card-stacked">
          <div className="card-content">
            <p>{ pokemon.name }</p>
            <p><small>{ formatDate(pokemon.created) }</small></p>
          </div>
        </div>
      </div> 
    </div>
  );
}
  
export default PokemonCard;
````

---

## Hook personnalisés

> Fonction JavaScript dont le nom commence par ``use``, cette fonction peut être appelée par d'autres Hooks.

---

## Les routes

> il faut installer la librairie ``react-router-dom`` pour ajouter la navigation dans le DOM du navigateur, car React ne possède pas de système de navigation par defaut
> Le Router depuis App :

````typescript
import React, { FunctionComponent } from 'react';
import PokemonList from './pages/pokemon-list';
import { BrowserRouter as Router, Link, Switch, Route } from 'react-router-dom';
import PokemonDetail from './pages/pokemon-detail';

const App: FunctionComponent = () => {
     
     return (
         <Router>
              <div>
                    { /*La barre de navigation commun a toutes les pages*/ }
                    <nav>
                         <div className="nav-wrapper teal">
                              <Link to="/" className="brand-logo center">Pokédex</Link>
                         </div>
                    </nav>
                    { /*Le système de gestion des routes de notre application*/ }
                    <Switch>
                         <Route exact path="/" component={ PokemonList }/>
                         <Route exact path="/pokemons" component={ PokemonList }/>
                         <Route exact path="/pokemon/:id" component={ PokemonDetail }/>
                         <Route component={ PageNotFound }/>
                    </Switch>
              </div>
         </Router>
     )
};

export default App;
````

> route dans le composant PokemonDetail

````typescript
import React, { FunctionComponent, useState, useEffect } from 'react';
import { RouteComponentProps, Link } from 'react-router-dom';
import Pokemon from '../models/pokemon';
import POKEMONS from '../models/mock-pokemon';
import formatDate from '../helpers/format-date';
import formatType from '../helpers/format-type';

interface Params { id: string };

const PokemonsDetail: FunctionComponent<RouteComponentProps<Params>> = ({ match }) => {
    const [pokemon, setPokemon] = useState<Pokemon | null>(null);

    useEffect(() => {
        POKEMONS.forEach(pokemon => {
            if (match.params.id === pokemon.id.toString()) {
                setPokemon(pokemon);
            }
        });
        
    }, [match.params.id]);

    return (
        <div>
            { pokemon ? (
                <div className="container_wrapper"> 
                <div className="wrapper">     
                    <div id="aspicot">{ pokemon.name }</div>
                    <div id="image"><img src={ pokemon.picture } alt=""/></div>
                    <div id="name">Nom</div>
                    <div id="name-pokemon">{ pokemon.name }</div>
                    <div id="pdv">Point de vie</div>
                    <div id="pdv-pokemon">{ pokemon.hp }</div>
                    <div id="typeP">Types</div>
                    <div id="type-pokemon">{ pokemon.types.map((type)=>(
                        <span key={ type } style={{ backgroundColor: formatType(type), borderRadius: '20px', padding:'0.5em' }}>{ type }</span>
                    )) }
                    </div>
                    <div id="date">Date de création</div>
                    <div id="date-pokemon">{ formatDate(pokemon.created) }</div>
                    <div id="retour">
                        <Link to="/">Retour</Link>
                    </div>
                </div>
            </div>
             ) : (
                <h1>Le pokémon demandé nexiste pas !</h1>)

            }
        </div>

        
    );
    }

````

Utilisation du Hook ``useHistory`` pour faire les redirections. Il nous donne accès à l'objet représentant l'historique du navigateur. L'autre possibilité est d'utilser ``Link`` (useHistory et Link font la même chose à savoir rediriger, mais l'objet de useHistory offre plus de possiblilité en terme de méthode comme par exemple goback)

````typescript
import React, { FunctionComponent, useState } from 'react';
import Pokemon from '../models/pokemon';
import './pokemon-card.css';
import formatDate from '../helpers/format-date';
import formatType from '../helpers/format-type';
import { useHistory } from 'react-router-dom';
  
type Props = {
  pokemon: Pokemon,
  borderColor?: string
};
  
const PokemonCard: FunctionComponent<Props> = ({ pokemon, borderColor = '#009688' }) => {
    
  const [color, setColor] = useState<string>();
  const history = useHistory();

  const showBorder = () => {
      setColor(borderColor);
  };  

  const hideBorder = () => {
      setColor('#f5f5f5'); // on remet la bordure en gris.s
  }; 
  
  const goToPokemon = (id: number) => {
    history.push(`/pokemon/${id}`);
  }

  return (
    <div className="col s6 m4" onClick={ () => goToPokemon(pokemon.id) } onMouseEnter={ showBorder } onMouseLeave={ hideBorder }>
      <div className="card horizontal" style={{ borderColor: color }}>
        <div className="card-image"> 
          <img src={ pokemon.picture } alt={ pokemon.name }/>
        </div>
        <div className="card-stacked">
          <div className="card-content">
            <p>{ pokemon.name }</p>
            <p><small>{ formatDate(pokemon.created) }</small></p>
            { pokemon.types.map(type => (
              <span key={ type } className={formatType(type)}>{ type }</span>
            )) }
          </div>
        </div>
      </div> 
    </div>
  );
}
  
export default PokemonCard;
````

gérer les erreurs 404

````typescript
import React, { FunctionComponent } from 'react';
import { Link } from 'react-router-dom';
  
const PageNotFound: FunctionComponent = () => {
  
  return (
    <div className="center">
      <img src="http://assets.pokemon.com/assets/cms2/img/pokedex/full/035.png" alt="Page non trouvée"/>
      <h1>Hey, cette page n'existe pas !</h1> 
      <Link to="/" className="waves-effect waves-teal btn-flat">
        Retourner à l'accueil
      </Link>
    </div>
  );
}
  
export default PageNotFound;
````

---

## Les formulaires

````html
 <label htmlFor="hp">Point de vie</label> <!-- for devient htmlFor en jsx -->
````

On peut créér des formulaires de deux manières diférentes :

Composant controlés         | Composant non controlés
--- | ---
State                       | DOM
Tous les formulaires        |  Seulement les petits formulaires
Le plus utilisé             | Le moins utilisé

> Evènements des formulaires React avec le typage de typeScript :

````typescript
// Tous les événements des éléments de formulaire sont du type, où T est le type d'élément HTML
React.ChangeEvent<T>

React.ChangeEvent<HTMLInputElement>
React.ChangeEvent<HTMLTextAreaElement>
React.ChangeEvent<HTMLInputSelect>
````

````typescript
const handlerInputChange = (e: React.ChangeEvent<HTMLInputElement>) => {
      const fieldName: string = e.target.name;
      const fieldValue: string = e.target.value;
      const newField: Field = { [fieldName]: { value: fieldValue }};

      setForm({ ...form, ...newField }) // fusion de deux objets. Si des proprietées sont en double c'est celles de l'objet de droite qui sont gardées.
    };

````

## Requêtes HTTP

Nous pouvons simuler une API REST grace à la librairie json-server

````typescript
npm install -g json-server
// puis création d'un fichier db.json
// puis lancement du server json pour simuler l'API REST
json-server --watch src/models/db.json --port=3001
// nous pouvons pour nous faciliter la vie en rajoutant un script dans package.json
"start:api": "json-server --watch src/models/db.json --port=3001",
npm run start:api

// !! avoir : pourquoi npm start et npm run start:api ?

````

---

## Annexes

Il existe deux façons courantes de configurer une nouvelle application React:

- L'utilitaire CLI, create-react-app : Facebook fournit un utilitaire de ligne de commande appelé create-react-app qui configure automatiquement un nouveau projet. create-react-app utilise sous le capot par webpack et babel.

````shell script
npx create-react-app my-app
cd my-app
npm start
# http://localhost:3000/
````

- Un bundler JavaScript, comme Webpack : Un bundler combine tous vos fichiers sources JavaScript dans un seul fichier, qui peut ensuite être inclus dans une balise script dans une page HTML. Les applications React sont le plus souvent construites avec Webpack et Babel, et npm en tant que gestionnaire de packages. **ATTENTION** pour webpack voir plutôt la note `webpack.md` pour une approche plus moderne.

````shell script
mkdir react-app
cd react-app
npm init

npm install --save-dev webpack webpack-cli webpack-dev-server
````

Ajouter au fichier `package.json` les deux commandes dans la partie `scripts`. Le script `start:dev` démarrera notre serveur de développement. Le script `build` enregistrera un seul fichier `.js`

````json
{
  ...
  "scripts": {
    "build": "webpack",
    "start:dev": "webpack serve --open 'Chrome'"
  },
  ...
}
````

Création du fichier `webpack.config.js`

````javascript
const path = require('path');

module.exports = {
  mode: "production",
  entry: {
    app: "./index.js"
  },
  output: {
    filename: "[name].bundle.js",
    path: path.resolve(__dirname, "dist")
  }
};
````

``ATTENTION`` il se peut que webpack considère que les fichiers sont générés à la racine du server et non dans le dossier `dist`.

````HTML
<body>
        <script src="/polyfill.bundle.js"></script>
        <script src="/app.bundle.js"></script>
</body>
````

---

Manière intéressante d'insérer du style css dans le jsx

````javascript
import React from 'react'
import ReactDOM from 'react-dom'

function App() {
  const style = {
    padding: '40px',
    textAlign: 'center',
  }

  return <div style={style}>Welcome to React!</div>
}

ReactDOM.render(<App />, document.querySelector('#app'))
````
