# React

<!-- 1. [Il existe deux types de composant dans REACT](#il-existe-deux-types-de-composant-dans-REACT) -->
1. [Deux types de composant](#2components)
2. [JSX](#jsx)
3. [Props](#props)
4. [Propriétées Calculées](#propsCalc)
5. [Hooks](#hooks)
6. [Routes](#routes)
7. [Routes Privées](#privatesRoutes)
8. [Les hooks "useLocation" et "useParams"](#useLocation)
9. [Authentification](#auth)
10. [Formulairess](#forms)
11. [Requêtes HTTP](#requetesHTTP)
12. [API de Context](#context)
13. [Redux](#redux)
14. [Annexes](#annexes)

Création d'une app React sans ``create react app`` ni de ``CDN`` (voir en annexe pour plus de détails).

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

## Il existe deux types de composant dans REACT <a name="2components"></a>

> Les noms des composants commencent toujours par une majuscule

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

## Les Hooks <a name="hooks"></a>

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

### Lazy Lazy initialisation

> Si l’on a besoin d'une valeur qu’une seul fois il est possible de passer une fonction fléchée à  React.useState(() => fonctionLongue()). La fonction longue ne sera alors appelé qu’une seule fois ce qui améliorera les performances.

````javascript
import * as React from 'react'

function Login({initialEmail = ''}) {
  const [email, setEmail] = React.useState(
    () => window.localStorage.getItem('email') || initialEmail,
  )
  const [error, setError] = React.useState(false)
  const handleChange = event => {
    setEmail(event.target.value)
    setError(!event.target.value.includes('@'))
    window.localStorage.setItem('email', event.target.value)
  }
  return (
    <div>
      <form>
        <label>Entrez votre email : </label>
        <input value={email} onChange={handleChange} />
      </form>
      <div style={{color: error ? 'red' : ''}}>
        Votre {email} est {error ? 'non valide' : 'valide'}{' '}
      </div>
    </div>
  )
}

function App() {
  return <Login initialEmail="example@example.com" />
}

export default App
````

> Les hooks permettent de rajouter des méthodes de  `cycle de vie` à un composants de fonction et se nomme `useEffect`. `useEffect` sera exécuté après le rendu !

Il y a trois méthodes `de cycle de vie` d'un composant dans React :

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

### Dépendances d’effets

````javascript
const [name, setName] = React.useState()
React.useEffect(() => {
  //se déclanche uniquement quand name change de valeur via setName
}, [name])
````

### Dépendances d’effets sur props

````javascript
import * as React from 'react'

function Login({initialEmail = ''}) {
  const [email, setEmail] = React.useState(
    () => window.localStorage.getItem('email') || initialEmail,
  )
  const [password, setPassword] = React.useState('')
  const handleChange = async event => setEmail(event.target.value)
  const handlePasswordChange = async event => setPassword(event.target.value)

  React.useEffect(() => {
    window.localStorage.setItem('email', email)
    console.log('useEffect email a changé')
  }, [email])
  React.useEffect(() => {
    window.localStorage.setItem('email', initialEmail)
    console.log('useEffect initialEmail a changé')
  }, [initialEmail])
  return (
    <div>
      <form>
        <label>Entrez votre email : </label>
        <input value={email} onChange={handleChange} />
        <label>Password : </label>
        <input value={password} onChange={handlePasswordChange} />
      </form>
    </div>
  )
}

function App() {
  const [count, setCount] = React.useState(0)
  React.useEffect(() => {
    const interval = setInterval(() => {
      setCount(count => count + 1)
    }, 5000)
    return () => clearInterval(interval)
  }, [])
  return <Login initialEmail={`example-${count}@example.com`} />
}
export default App

````

## 3 règles à respecter avec les Hooks

- On ne doit pas appeler les hooks à l'intérieur d'une boucle ou d'une condition. Il faut utiliser les hooks à la racine du composant fonction.
- On appel les hooks uniquement depuis des composants de fonction ! Exception : on peut appeler un hook depuis un autre composant que l'on a créé (hooks personnalisé).
- Quand on utilise la méthode du hook (ex. setName...) la valeur du state est remplacée complètement et non fusionnée !

## `useRef`

La différence entre `useState` et `useRef` est que `useState` provoque un nouveau rendu, `useRef` ne le fait pas.

> Fondamentalement, nous utilisons UseState dans les cas où la valeur de l'état doit être mise à jour avec un nouveau rendu.
lorsque vous voulez que vos informations persistent pendant la durée de vie du composant, vous utiliserez UseRef car ce n'est tout simplement pas pour travailler avec le re-rendu.

> la fonction entière est exécutée chaque fois qu'elle est restituée. Ce qui signifie que les variables initialisées avec un simple let x = 5;dans le corps de la fonction seront réinitialisées à chaque rendu, les réinitialisant. C'est la raison pour laquelle nous avons besoin de crochets comme useRef, cela donne une référence à une valeur qui persiste entre les rendus

---

## JSX et le DOM virtuel <a name="jsx"></a>

Exemple de React sans le `JSX`

````html
<!-- Manipuler le DOM avec React Sans JSX-->

<html>
  <head>
    <script src="https://unpkg.com/react@17.0.2/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@17.0.2/umd/react-dom.development.js"></script>
  </head>
  <body>
    <div id="root"></div>
    <script>
      const rootEl = document.getElementById('root')
      const divEl = React.createElement('div', {
        className: 'rootContainer',
        children: [
          React.createElement('h1', null, 'Bienvenue'),
          React.createElement('h2', null, 'Commencez ici'),
          React.createElement(
            'p',
            null,
            'Lorem Ipsum is simply dummy text of the printing and typesetting industry',
          ),
        ],
      })
      ReactDOM.render(divEl, rootEl)
    </script>
  </body>
</html>
````

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

toggle les classes et css

````javascript
import { useState } from "react";
import Item from "./Components/Item/Item";

function App() {
  const [toggle, setToggle] = useState(false);

  const changeState = () => {
    setToggle(!toggle);
  };

  return (
    <div className="App">
      <div className={toggle ? "box animated" : "box"}></div>
      <button onClick={changeState}>Change state</button>
    </div>
  );
}

export default App;
````

````javascript
function Login({initialEmail = ''}) {
  const [email, setEmail] = React.useState(initialEmail)
  const [error, setError] = React.useState(false)
  const handleChange = event => {
    setEmail(event.target.value)
    setError(!event.target.value.includes('@'))
  }
  return (
    <div>
      <form>
        <label>Entrez votre email : </label>
        <input value={email} onChange={handleChange} />
      </form>
      <div style={{color: error ? 'red' : ''}}>
        Votre {email} est {error ? 'non valide' : 'valide'}{' '}
      </div>
    </div>
  )
}

function App() {
  return <Login initialEmail="example@example.com" />
}

export default App
````

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

## Les props <a name="props"></a>

> Pour passer une props à un composant il faut l'ajouter en paramètre de la fonction du composant.
> On passe des props depuis un composant parent.
> Les props sont les données en entrées d'un composant.
> Les props sonts des objets.

````javascript
  function Hello (props) {
    return <h1> Hello ${props.name} </h1>
  }
  // Appel
  <Hello name="thierry"/>

  // Avec la destructuration
  function Hello ({name}) {
    return <h1> Hello {name} </h1>
  }
  // Appel
  <Hello name="thierry"/>


````

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

### `PropTypes`

Nous pouvons utiliser des extensions JavaScript comme Flow ou TypeScript pour valider les types de notre application. Mais même si vous ne les utilisez pas, React possède ses propres fonctionnalités de validation de types.

````javascript
import PropTypes from 'prop-types';

const Paragraphe = ({text, author}) => {
        return (
          <p className="paragraphe">
            {text} : {author}
          </p>
        )
      }

      Paragraphe.propTypes = {
        text: PropTypes.number,
        author: PropTypes.string,
        // text: PropTypes.string.isRequired,
        // author: PropTypes.string.isRequired
      }
````

---

## les propriétées calculées <a name="propsCalc"></a>

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

## Hooks personnalisés <a name="hooksPerso"></a>

> Fonction JavaScript dont le nom commence par ``use``, cette fonction peut être appelée par d'autres Hooks.

`useDimension.js`

````javascript
export default function useDimension() {

    const [dimension, setDimension] = useState();

    useEffect(() => {
        window.addEventListener('resize', resizeFunc);

        function resizeFunc() {
            setDimension(window.innerWidth);
        }

        resizeFunc();

        return () => {
            window.addEventListener('resize', resizeFunc)
        }
    }, [])

    return dimension;
}
````

`app.js`

````javascript
import useDimension from './useDimension';

function App() {
  
  const browserWidth = useDimension();
  console.log(browserWidth);

  if(browserWidth > 772) {
    console.log("grand écran");
  } else {
    console.log("petit écran");
  }

  return (
    <div className="App">
      
    </div>
  );
}

export default App;
````

---

## Les routes <a name="routes"></a>

> il faut installer la librairie ``react-router-dom`` pour ajouter la navigation dans le DOM du navigateur, car React ne possède pas de système de navigation par defaut
> Le Router depuis App :

````typescript
import React, { FunctionComponent } from 'react';
import PokemonList from './pages/pokemon-list';
import { BrowserRouter as Router, Link, Switch, Route } from 'react-router-dom';
import PokemonDetail from './pages/pokemon-detail';

const App: FunctionComponent = () => {
     
     return (
       { /*<Router basename={ process.env.PUBLIC_URL }> */ }
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

Utilisation du Hook ``useHistory`` pour faire les redirections. Il nous donne accès à l'objet représentant l'historique du navigateur. L'autre possibilité est d'utilser ``Link`` (useHistory et Link font la même chose à savoir rediriger, mais l'objet de useHistory offre plus de possiblilité en terme de méthode comme par exemple goback). A voir aussi `navlinks`

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

## Routes Privées <a name="privatesRoutes"></a>

Avant:

````javascript
import React, { FunctionComponent } from 'react'
import { BrowserRouter as Router, Switch, Route } from 'react-router-dom';
import './App.css';
import Home from './components/home';
import Login from './components/login';
import Registration from './components/registration';
import Choose_Game from './components/choose_game';
import Game from './components/game';
import Admin from './components/admin';
import PageNotFound from './components/pageNotFoung';

const App: FunctionComponent = () => {

  return (
    
       <Router>
       <div>
             { /*Création d'une div pour gérer l'opacitée*/ }
             <div id="opacity"></div>
             { /*Le système de gestion des routes de notre application*/ }
             <Switch>
                  <Route exact path="/" component={ Home }/>
                  <Route exact path="/login" component={ Login }/>
                  <Route exact path="/registration" component={ Registration }/>
                  <Route exact path="/choose_game" component={ Choose_Game }/>
                  <Route exact path="/game" component={ Game }/>
                  <Route exact path="/admin" component={ Admin }/>
                  <Route component={ PageNotFound }/>
             </Switch>
       </div>
       </Router>
       
  );
}

export default App;

````

`App.tsx`

````javascript
import React, { FunctionComponent } from 'react'
import { BrowserRouter as Router, Switch, Route } from 'react-router-dom';
import './App.css';
import Home from './components/home';
import Login from './components/login';
import Registration from './components/registration';
import Choose_Game from './components/choose_game';
import Game from './components/game';
import Admin from './components/admin';
import PageNotFound from './components/pageNotFoung';
import PrivateRoute from './components/privateRoute';

const App: FunctionComponent = () => {

  return (
    
       <Router>
       <div>
             { /*Création d'une div pour gérer l'opacitée*/ }
             <div id="opacity"></div>
             { /*Le système de gestion des routes de notre application*/ }
             <Switch>
                  <Route exact path="/" component={ Home }/>
                  <Route exact path="/login" component={ Login }/>
                  <PrivateRoute exact path="/registration" component={ Registration }/>
                  <PrivateRoute exact path="/choose_game" component={ Choose_Game }/>
                  <PrivateRoute exact path="/game" component={ Game }/>
                  <PrivateRoute exact path="/admin" component={ Admin }/>
                  <Route component={ PageNotFound }/>
             </Switch>
       </div>
       </Router>
       
  );
}

export default App;
````

`privateRoute.tsx`

````javascript
import React from 'react';
import { Route, Redirect } from 'react-router-dom';
import AuthenticationService from '../services/authentication-services';
  
const PrivateRoute = ({ component: Component, ...rest }: any) => (
  <Route {...rest} render={(props) => {
    const isAuthenticated = AuthenticationService.isAuthenticated;
    if (!isAuthenticated) {    
      // return <Redirect to={{ pathname: '/login' }} />
      return <Redirect to={{ pathname: '/login' }} />
    }
  
    return <Component {...props} />
  }} />
);
  
export default PrivateRoute;
````

---

## Les hooks "useLocation" et "useParams" <a name="useLocation"></a>

C'est hooks ont été créés par l'équipe de React router

`App.js`

````javascript
import './App.css';
import Accueil from './Components/Accueil'; 
import Projets from './Components/Projets'; 
import Contacts from './Components/Contacts';
import Nav from './Components/Nav/Nav' ;
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';


function App() {
  return (
    <>
      {/* <Accueil/> */}
      <Router>
        <Nav/>
        <Switch>
          <Route exact path="/" component={ Accueil } />
          <Route exact path="/Projets" component={ Projets } />
          <Route exact path="/Projets/:slug" component={ Projets } />
          <Route exact path="/Contacts" component={ Contacts } />
          <Route component={ () => <div>ERREUR 404 :(</div> }/>
        </Switch>
      </Router>
    </>
  );
}

export default App;
````

`Projets.js`

````javascript
import React from 'react';
import { useParams } from 'react-router-dom';


export default function Projets() {

    //http://localhost:3000/Projets/toto ici le slug c'est toto
    const { slug } = useParams();
    console.log(slug); // toto

    return (
        <>
            <h1>Section PROJETS</h1>
            <p>{ slug }</p>
        </>
    )
}
````

useLocation

`Accueil.js`

````javascript
import React from 'react';
import { Link } from 'react-router-dom';

export default function Accueil() {
    return (
        <>
            <h1>Bienvenue ACCUEIL</h1>
            <Link
            to={{
                pathname: "/Contacts",
                state: {
                    txt: "voilà des données"
                }
            }}
            >
            Aller à la section Contacts
            </Link>
        
        </>
    )
}
````

`Contacts.js`

````javascript
import React from 'react'
import { useLocation } from 'react-router-dom'

export default function Contacts() {

    const location = useLocation();
    console.log(location); 
    // renvoi un objet contenant un "state" nous permettant de passer des données
    // lorque l'on navigue dans notre application
    console.log(location.state.txt); // voilà des données
    return (
        <h1>Section CONTACTS</h1>
    )
}

````

---

## Authentification <a name="auth"></a>

`authentication-services.ts`

````javascript
import axios from 'axios';

export default class AuthenticateService {
    static isAuthenticated = false;
    static allLogin: any[];
    static privilege: string;

    static getAllLogins() {
          return axios
                .get("/loginAll")
                .then((allLogin) => {this.allLogin = allLogin.data;
                                     console.log('in getAllLogin from AUthenticated class', this.allLogin);
                                     return this.allLogin;
                                    })
                .catch((err) => console.log(err));
    }

    static async verifyLogin(pseudo: string, pwd: string): Promise<boolean> {
        let result = await this.getAllLogins();
        for(let i=0; i<this.allLogin.length; i++) {
                    if(this.allLogin[i].pseudo === pseudo && this.allLogin[i].password === pwd) {
                        this.isAuthenticated = true;
                        this.privilege = this.allLogin[i].privileges;
                        console.log('authenticated ===', this.isAuthenticated);
                        console.log('privilege ===', this.privilege);
                    }
        }
        return this.isAuthenticated;
    }
}
````

`login.tsx`

````javascript
import  React, { useState } from 'react';
import './login.css';
import { useHistory } from 'react-router-dom';

import AuthenticateService from '../services/authentication-services';

const Login = () => {

    const [allLogin, setAllLogin]   = useState<any>();
    const [pseudo, setPseudo]       = useState<string>("");
    const [pwd, setPwd]             = useState<string>("");
    const [message, setMessage]     = useState<string>("");

    const history = useHistory();
    console.log("history ===", history);

    const handleSubmit = (e: React.FormEvent<HTMLFormElement>): void => {
        e.preventDefault();
        console.log("allLogin === ", allLogin);
        
        if(!pseudo || !pwd) {
            setMessage('veuillez remplir tous les champs');
            return;
        }

        AuthenticateService.verifyLogin(pseudo, pwd).then(res => {console.log("res in login.tsx ===", res);
                                                                  if(!res) {
                                                                    setMessage("peudo et/ou mot de passe incorrect");
                                                                    return;
                                                                  }
                                                                  history.push('/choose_game', { pseudo: pseudo,
                                                                                                 privilege: AuthenticateService.privilege
                                                                                                });
                                                                 });

    }
    /*
    * vérification au keyup si tous les champs sont remplis on supprime le message 'veuillez remplir tous les champs'
    */
    const handleKeyUp = (): void => {
        console.log('key up function enter :)');
        
        if(pseudo !== "" && pwd !== "") {
            setMessage("");
        }
    }

    return (
        <div>
            <form id="login" onSubmit={ e=> handleSubmit(e) } onKeyUp={ handleKeyUp }>
                <h1>LOGIN</h1>
                <div id="message_login">{ message }</div>
                <input
                    id="pseudo"
                    name="pseudo"
                    type="text"
                    onChange={ (e) => setPseudo(e.target.value) }
                    spellCheck="false"
                    placeholder="Pseudo"
                    autoComplete="off"
                />
                <input 
                    id="pwd"
                    name="pwd"
                    type="password"
                    onChange={ (e) => setPwd(e.target.value) }
                    placeholder="Password"
                />
                <input type="submit"   name="submit" value="Login"/>
                <a href="/registration">Create an Account</a>
            </form>
        </div>
    );
}

export default Login;
````

---

## Les formulaires <a name="forms"></a>

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

````typescript
const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
      e.preventDefault();
      console.log(form);
      const isFormValid = validateForm();
      
      if(isFormValid) {
        history.push(`/pokemon/${ pokemon.id }`);
      }
}
return (
    <form onSubmit={ e=> handleSubmit(e)}>
    //....
);        
````

---

Exemple :

````javascript
const handleSubmit = (e) => {
    e.preventDefault();
    alert(`bonjour ${e.target.elements.emailInput.value}`); //pour récupèrer la valeur de l'email
  }

  return (
    <form onSubmit={handleSubmit}>
      <label>
        Adresse email :
        <input type="text" name="emailInput" />
      </label>
      <input type="submit" value="Connexion" />
    </form>
  )
}
````

---

Exemple :

````typescript
import React, { FunctionComponent, useState } from 'react';
import { useHistory } from 'react-router-dom';
import Pokemon from '../models/pokemon';
import formatType from '../helpers/format-type';
import './pokemon-form.css';
  
type Field = {
  value?: any,
  error?: string,
  isValid?: boolean
}

type Form = {
  name: Field,
  hp: Field,
  cp: Field,
  type: Field
};

type Props = {
  pokemon: Pokemon
};

const PokemonForm: FunctionComponent<Props> = ({ pokemon }) => {
  
  const [form, setForm] = useState<Form>(
    {
      name: { value: pokemon.name,
              isValid: true },
      hp:   { value: pokemon.hp,
              isValid: true },
      cp:   { value: pokemon.cp,
              isValid: true },
      type: { value: pokemon.types,
              isValid: true }
    }
    );

    const history = useHistory();
    
    const types: string[] = [
      'Plante', 'Feu', 'Eau', 'Insecte', 'Normal', 'Electrik',
      'Poison', 'Fée', 'Vol', 'Combat', 'Psy'
    ];
    
    const whichType = (type: string): boolean => {
      return form.type.value.includes(type);
    }

    const handlerInputChange = (e: React.ChangeEvent<HTMLInputElement>) => {
      const fieldName: string = e.target.name;
      const fieldValue: string = e.target.value;
      const newField: Field = { [fieldName]: { value: fieldValue }};

      setForm({ ...form, ...newField })
    };

    const selectType = (type: string, e: React.ChangeEvent<HTMLInputElement>): void => {
      const checked = e.target.checked;
      let newField: Field;

      if(checked) {
        // Si l'utilisateur coche un type, à l'ajoute à la liste des types du pokémon.
        const newTypes: string[] = form.type.value.concat([type]);
        newField = { value: newTypes };
      } else {
        // Si l'utilisateur décoche un type, on le retire de la liste des types du pokémon.
        const newTypes: string[] = form.type.value.filter((currentType: string) => currentType !== type) ;
        newField = { value: newTypes };
      }

      setForm({...form, ...{ type: newField }});
    }

    const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
      e.preventDefault();
      console.log(form);
      const isFormValid = validateForm();
      
      if(isFormValid) {
        history.push(`/pokemon/${ pokemon.id }`);
      }
    }

    const validateForm = () => {
      let newForm: Form = form;
      
      // Validator name
      if(!/^[a-zA-Zàéè ]{3,25}$/.test(form.name.value)) {
        const errorMsg: string = 'Le nom du pokémon est requis (1-25).';
        const newField: Field = { value: form.name.value, error: errorMsg, isValid: false };
        newForm = { ...newForm, ...{ name: newField } };
      } else {
        const newField: Field = { value: form.name.value, error: '', isValid: true };
        newForm = { ...newForm, ...{ name: newField } };
      }
  
      // Validator hp
      if(!/^[0-9]{1,3}$/.test(form.hp.value)) {
        const errorMsg: string = 'Les points de vie du pokémon sont compris entre 0 et 999.';
        const newField: Field = {value: form.hp.value, error: errorMsg, isValid: false};
        newForm = { ...newForm, ...{ hp: newField } };
      } else {
        const newField: Field = { value: form.hp.value, error: '', isValid: true };
        newForm = { ...newForm, ...{ hp: newField } };
      }
  
      // Validator cp
      if(!/^[0-9]{1,2}$/.test(form.cp.value)) {
        const errorMsg: string = 'Les dégâts du pokémon sont compris entre 0 et 99';
        const newField: Field = {value: form.cp.value, error: errorMsg, isValid: false};
        newForm = { ...newForm, ...{ cp: newField } };
      } else {
        const newField: Field = { value: form.cp.value, error: '', isValid: true };
        newForm = { ...newForm, ...{ cp: newField } };
      }
  
      setForm(newForm);
      return newForm.name.isValid && newForm.hp.isValid && newForm.cp.isValid;
    }

    const isTypesValid = (type: string): boolean => {
      // Cas n°1: Le pokémon a un seul type, qui correspond au type passé en paramètre.
      // Dans ce cas on revoie false, car l'utilisateur ne doit pas pouvoir décoché ce type (sinon le pokémon aurait 0 type, ce qui est interdit)
      if (form.type.value.length === 1 && whichType(type)) {
        return false;
      }
      
      // Cas n°1: Le pokémon a au moins 3 types.
      // Dans ce cas il faut empêcher à l'utilisateur de cocher un nouveau type, mais pas de décocher les types existants.
      if (form.type.value.length >= 3 && !whichType(type)) { 
        return false; 
      } 
      
      // Après avoir passé les deux tests ci-dessus, on renvoie 'true', 
      // c'est-à-dire que l'on autorise l'utilisateur à cocher ou décocher un nouveau type.
      return true;
    }

  return (
    <form onSubmit={ e=> handleSubmit(e)}>
      
            <div>
              <img src={pokemon.picture} alt={form.name.value} style={{width: '250px', margin: '0 auto'}}/>
            </div>      
                {/* Pokemon name */}
                <div>
                  <label htmlFor="name">Nom</label>
                  <input id="name" name="name" type="text" value={ form.name.value } onChange={ e => handlerInputChange(e) }></input>
                  { form.name.error && 
                  <div>
                    { form.name.error }
                  </div>
                  }
                </div>
                {/* Pokemon hp */}
                <div>
                  <label htmlFor="hp">Point de vie</label>
                  <input id="hp" name="hp" type="number" value={ form.hp.value} onChange={ e => handlerInputChange(e) }></input>
                  { form.hp.error && 
                  <div>
                    { form.hp.error }
                  </div>
                  }
                </div>
                {/* Pokemon cp */}
                <div >
                  <label htmlFor="cp">Dégâts</label>
                  <input id="cp" name="cp" type="number" value={ form.cp.value} onChange={ e => handlerInputChange(e) }></input>
                  { form.cp.error && 
                  <div>
                    { form.cp.error }
                  </div>
                  }
                </div>
                {/* Pokemon types */}
                <div>
                  <label>Types</label>
                  {types.map(type => (
                    <div key={type} style={{ marginBottom: '10px' }}>
                      <label>
                        <input id={ type } type="checkbox" checked={whichType(type)} disabled={ !isTypesValid(type) } onChange={e => selectType(type, e)}></input>
                        <span id="type" style={{ backgroundColor: formatType(type) }}>{ type }
                          {/* <p className={formatType(type)}>{ type }</p> */}
                          
                        </span>
                      </label>
                    </div>
                  ))}
                </div>
              
              <div>
                {/* Submit button */}
                <button type="submit">Valider</button>
              </div>
            
    </form>
  );
};
   
export default PokemonForm;
````

> Autre exemple:

````javascript
import React, { useState } from "react";
import { useDispatch } from "react-redux";
import "./Form.css";

export default function Form() {
  const [article, setArticle] = useState({
    title: "",
    body: "",
  });

  const dispatch = useDispatch();

  const handleForm = (e) => {
    e.preventDefault();
  };

  const handleInputs = (e) => {

    if (e.target.classList.contains("inp-title")) {
        const newObjState = { ...article, title: e.target.value };
        setArticle(newObjState);
    }
    else if (e.target.classList.contains("inp-body")) {
        const newObjState = { ...article, body: e.target.value };
        setArticle(newObjState);
    }
  };

  return (
    <>
      <h1 className="title-form">Écrivez un article</h1>

      <form onSubmit={handleForm} className="container-form">
        <label htmlFor="title">Titre</label>
        <input
          onChange={handleInputs}
          value={article.title}
          type="text"
          id="title"
          placeholder="Entrez votre nom"
          className="inp-title"
        />

        <label htmlFor="article">Votre article</label>
        <textarea
          onChange={handleInputs}
          value={article.body}
          id="article"
          placeholder="Votre article"
          className="inp-body"
        ></textarea>

        <button>Envoyez l'article</button>
      </form>
    </>
  );
}

````

## Requêtes HTTP <a name="requetesHTTP"></a>

Nous pouvons simuler une API REST grace à la librairie json-server
(C'est comme une mini base de donnée avec un fichier)

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

Utilisation de l'API fetch

````typescript
import React, { FunctionComponent, useState, useEffect } from 'react';
import Pokemon from '../models/pokemon';
import POKEMONS from '../models/mock-pokemon';
import PokemonCard from '../components/pokemon-card';
import './pokemon-list.css';
  
const PokemonList: FunctionComponent = () => {
  const [pokemons, setPokemons] = useState<Pokemon[]>([]);
  
  useEffect(() => {
   //  setPokemons(POKEMONS);
   fetch('http://localhost:3001/pokemon')
   .then(response => response.json())
   .then((pokemons) => {
      setPokemons(pokemons)
   });
  }, []);
  
  return (
    
  <div>
      <h1>Pokédex</h1>
      <div>
         <div className="container">
            { pokemons.map((pokemon)=> (
               <PokemonCard key= { pokemon.id } pokemon= { pokemon } />      
            )) }
         </div>
      </div>
  </div>
  );
}
  
export default PokemonList;
````

````javascript
import React, { FunctionComponent, useState, useEffect } from 'react';
import { RouteComponentProps, Link } from 'react-router-dom';
import Pokemon from '../models/pokemon';
import POKEMONS from '../models/mock-pokemon';
import formatDate from '../helpers/format-date';
import formatType from '../helpers/format-type';
import './pokemon-detail.css';
// import { type } from 'os';

interface Params { id: string };

const PokemonsDetail: FunctionComponent<RouteComponentProps<Params>> = ({ match }) => {
    const [pokemon, setPokemon] = useState<Pokemon | null>(null);

    useEffect(() => {
        // POKEMONS.forEach(pokemon => {
        //     if (match.params.id === pokemon.id.toString()) {
        //         setPokemon(pokemon);
        //     }
        // });
        fetch(`http://localhost:3000/pokemons/${ match.params.id }`)
        .then(response => response.json())
        .then(pokemon => {
            if(pokemon.id) setPokemon(pokemon);
        });
        
    }, [match.params.id]);

    return ( 
      // .....
    );
    }
````

Factoriser du code dans un service : src/services/

````typescript
import Pokemon from "../models/pokemon";
 
export default class PokemonService {
 
  static getPokemons(): Promise<Pokemon[]> {
    return fetch('http://localhost:3001/pokemons')
      .then(response => response.json());
  }
 
  static getPokemon(id: number): Promise<Pokemon|null> {
    return fetch(`http://localhost:3001/pokemons/${id}`)
      .then(response => response.json())
      .then(data => this.isEmpty(data) ? null : data);
  }
 
  static isEmpty(data: Object): boolean {
    return Object.keys(data).length === 0;
  }
}

````

````typescript
import React, { FunctionComponent, useState, useEffect } from 'react';
import Pokemon from '../models/pokemon';
import PokemonCard from '../components/pokemon-card';
import './pokemon-list.css';
import PokemonService from '../services/pokemon-service';
  
const PokemonList: FunctionComponent = () => {
  const [pokemons, setPokemons] = useState<Pokemon[]>([]);
  
  useEffect(() => {
   PokemonService.getPokemons().then(pokemons => setPokemons(pokemons));
  }, []);
  
  return (  
      <div>
          <h1>Pokédex</h1>
          <div>
             <div className="container">
                { pokemons.map((pokemon)=> (
                   <PokemonCard key= { pokemon.id } pokemon= { pokemon } />      
                )) }
             </div>
          </div>
      </div>
  );
}
  
export default PokemonList;
````

````typescript
import React, { FunctionComponent, useState, useEffect } from 'react';
import { RouteComponentProps } from 'react-router-dom';
import PokemonForm from '../components/pokemon-form';
import Pokemon from '../models/pokemon';
import PokemonService from '../services/pokemon-service' ;

type Params = { id: string };
  
const PokemonEdit: FunctionComponent<RouteComponentProps<Params>> = ({ match }) => {
    
  const [pokemon, setPokemon] = useState<Pokemon|null>(null);
  
  useEffect(() => {
    PokemonService.getPokemon(+match.params.id).then(pokemon => setPokemon(pokemon));
  }, [match.params.id]);
    
  return (
    <div>
      { pokemon ? (
        <div className="row">
            <h2 className="header center">Éditer { pokemon.name }</h2>
            <PokemonForm pokemon={ pokemon }></PokemonForm>
        </div>
      ) : (
        <h4 className="center">Aucun pokémon à afficher !</h4>
      )}
    </div>
  );
}
  
export default PokemonEdit;
````

Gérer les erreurs HTTP

````typescript
import Pokemon from "../models/pokemon";
 
export default class PokemonService {
 
  static getPokemons(): Promise<Pokemon[]> {
    return fetch('http://localhost:3001/pokemons')
      .then(response => response.json())
      .catch(error => this.handleError(error));
  }
 
  static getPokemon(id: number): Promise<Pokemon|null> {
    return fetch(`http://localhost:3001/pokemons/${id}`)
      .then(response => response.json())
      .then(data => this.isEmpty(data) ? null : data)
      .catch(error => this.handleError(error));
  }
 
  static isEmpty(data: Object): boolean {
    return Object.keys(data).length === 0;
  }

  static handleError(error: Error): void {
    console.log(error);
  }
}
````

PUT

````typescript
static updatePokemon(pokemon: Pokemon): Promise<Pokemon> {
    return fetch(`http://localhost:3001/pokemons/${pokemon.id}`, {
      method: 'PUT',
      body: JSON.stringify(pokemon),
      headers: { 'Content-Type': 'application/json' }
    })
    .then(response => response.json())
    .catch(error => this.handleError(error));
  }
````

````typescript
const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
      e.preventDefault();
      console.log(form);
      console.log(pokemon);
      const isFormValid = validateForm();
      
      if(isFormValid) {
        pokemon.name = form.name.value;
        pokemon.hp = form.hp.value;
        pokemon.cp = form.cp.value;
        pokemon.types = form.type.value;
        PokemonService.updatePokemon(pokemon).then(() => history.push(`/pokemon/${ pokemon.id }`));
      }
    }
````

Avec json server on peut ajouter une option pour simuler un délai de répose du serveur

````json
"scripts": {
    "start": "react-scripts start",
    "start:api": "json-server --watch src/models/db.json --port=3001 --delay=500",
    "build": "react-scripts build"
  },
````

---

## API de Context <a name="context"></a>

L'API de context fait partie de REACT. Permet de partager des données dans toute l'application.

`\src\Context\ThemeContext.js`

````javascript
import React, { createContext, useState } from 'react';

export const ThemeContext = createContext();

const ThemeContextProvider = props => {

    const [theme, setTheme] = useState("Hello Context");

    return (
        <ThemeContext.Provider value={{ theme }}>
            { props.children }
        </ThemeContext.Provider>
    )

}
export default ThemeContextProvider;
````

`\src\App.js`

````javascript
import './App.css';
import Content from './Components/Content/Content';
import ThemeContextProvider from './Context/ThemeContext';

function App() {
  return (
    <div className="App">
      <ThemeContextProvider>
        <Content />
      </ThemeContextProvider>
    </div>
  );
}

export default App;

````

`\src\Components\Content\Content.js``

````javascript
import React, { useContext } from 'react';
import BtnToggle from '../BtnToggle/BtnToggle';
import './Content.css';
import { ThemeContext } from '../../Context/ThemeContext';

export default function Content() {

    const { theme } = useContext(ThemeContext);

    console.log(theme); // Hello Context

    return (
        <div className="container">
            <BtnToggle />
            <h1> Lorem ipsum dolor sit amet.</h1>
            <p className="content-txt">
            Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Nullam feugiat, turpis at pulvinar vulputate, erat libero tristique tellus, 
            </p>
        </div>
    )
}
````

`\src\Components\BtnToggle\BtnToggle.js`

````javascript
import React from 'react';
import './BtnToggle.css';

export default function BtnToggle() {
    return (
        <button className="btn-toggle">Toggle</button>
    )
}
````

---
Autre exemple :
Il est possible de passer des fonctions dans le context

`\src\index.css`

````css
body {
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen',
    'Ubuntu', 'Cantarell', 'Fira Sans', 'Droid Sans', 'Helvetica Neue',
    sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  transition: color 0.4s ease-in-out,
  background-color 0.4s ease-in-out;
}

code {
  font-family: source-code-pro, Menlo, Monaco, Consolas, 'Courier New',
    monospace;
}
.dark-body {
  background: #222;
  color: #f1f1f1;
}
````

`\src\Components\BtnToggle\BtnToggle.css`

````css
.btn-toggle {
    position: absolute;
    top: 10px;
    right: 10px;
    border-radius: 50%;
    width: 75px;
    height: 75px;
    display: flex;
    justify-content: center;
    align-items: center;
    outline: none;
}
.dark-btn {
    background: #222;
    color: #f1f1f1;
}
````

`\src\Context\ThemeContext.js`

````javascript
import React, { createContext, useState } from 'react';

export const ThemeContext = createContext();

const ThemeContextProvider = props => {

    const [theme, setTheme] = useState(false);

    const toggleTheme = () => {
        setTheme(!theme);
    }
    if (theme) {
        document.body.classList.add("dark-body");
    } else {
        document.body.classList.remove("dark-body");
    }

    return (
        <ThemeContext.Provider value={{ theme, toggleTheme }}>
            { props.children }
        </ThemeContext.Provider>
    )

}
export default ThemeContextProvider;
````

`\src\App.js`

````javascript
import './App.css';
import Content from './Components/Content/Content';
import ThemeContextProvider from './Context/ThemeContext';

function App() {
  return (
    <div className="App">
      <ThemeContextProvider>
        <Content />
      </ThemeContextProvider>
    </div>
  );
}

export default App;
````

`\src\Components\Content\Content.js`

````javascript
import React, { useContext } from 'react';
import BtnToggle from '../BtnToggle/BtnToggle';
import './Content.css';
import { ThemeContext } from '../../Context/ThemeContext';

export default function Content() {

    const { theme } = useContext(ThemeContext);

    console.log(theme);

    return (
        <div className="container">
            <BtnToggle />
            <h1> Lorem ipsum dolor sit amet.</h1>
            <p className="content-txt">
            Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Nullam feugiat, turpis at pulvinar vulputate, erat libero tristique tellus, nec bibendum odio risus sit amet ante. Aliquam erat volutpat. Nunc auctor. Mauris pretium quam et urna. 
            </p>
        </div>
    )
}
````

`\src\Components\BtnToggle\BtnToggle.js``

````javascript
import React, { useContext } from 'react';
import './BtnToggle.css';
import { ThemeContext } from '../../Context/ThemeContext';

export default function BtnToggle() {

    const { toggleTheme, theme } = useContext(ThemeContext);

    return (
        <button
        onClick={ toggleTheme }
        className={ theme ? "btn-toggle" : "btn-toggle dark-btn" }>
            { theme ? "LIGHT" : "DARK"}
        </button>
    )
}
````

---

## Redux <a name="redux"></a>

<https://react-redux.js.org/>

````shell script
  npm install redux
  npm install react-redux
````

> Création simple d'un store et d'un reducer

`App.js`

````javascript
import './App.css';

function App() {
  return (
    <div className="App">
      
    </div>
  );
}

export default App;

````

`index.js`

````javascript
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import { createStore } from 'redux';
import { Provider } from 'react-redux';
import CounterReducer from './Reducers/CounterReducer';

const Store = createStore(CounterReducer);

ReactDOM.render(
  <Provider store={ Store }>
    <App />
  </Provider>,
  document.getElementById('root')
);
````

`\src\Reducers\CounterReducer.js`

````javascript
const INITIAL_STATE = {
    count: 0
}

function CounterReducer(state = INITIAL_STATE, action) {
  
    return state;
}

export default CounterReducer;
````

> utilisation de `useSelector` pour se connecter au store depuis le composent Counter.js

`index.js`

````javascript
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import { createStore } from 'redux';
import { Provider } from 'react-redux';
import CounterReducer from './Reducers/CounterReducer';

const Store = createStore(CounterReducer);

ReactDOM.render(
  <Provider store={ Store }>
    <App />
  </Provider>,
  document.getElementById('root')
);
````

`App.js`

````javascript
import './App.css';
import Counter from './Components/Counter'

function App() {
  return (
    <div className="App">
      <Counter/>
    </div>
  );
}

export default App;
````

`\src\Components\Counter.js`

````javascript
import React from 'react';
import { useSelector } from 'react-redux';

export default function Counter() {

    const count = useSelector(state => state.count)

    return (
        <div>
            <h1>Les données : { count }</h1>
        </div>
    )
}
````

`\src\Reducers\CounterReducer.js`

````javascript
const INITIAL_STATE = {
    count: 10
}

function CounterReducer(state = INITIAL_STATE, action) {
  
    return state;
}

export default CounterReducer;
````

> Modification du state en utilisant useDispatch pour envoyer une Action

`\src\Components\Counter.js`

````javascript
import React from 'react';
import { useSelector, useDispatch} from 'react-redux';

export default function Counter() {

    const count = useSelector(state => state.count)
    const dispatch = useDispatch();

    const incrFunc = () => {
        dispatch({
            type: "INCR"
        })
    }
    const decrFunc = () => {
        dispatch({
            type: "DECR"
        })
    }

    return (
        <div>
            <h1>Les données : { count }</h1>
            <button onClick={ decrFunc }>-1</button>
            <button onClick={ incrFunc }>+1</button>
        </div>
    )
}
````

`\src\Reducers\CounterReducer.js`

````javascript
const INITIAL_STATE = {
    count: 10
}

function CounterReducer(state = INITIAL_STATE, action) {
  
    switch(action.type){
        case 'INCR': {
            return {
                ...state, // copie du state
                count: state.count +1
            }
        }
        case 'DECR': {
            return {
                ...state, // copie du state
                count: state.count -1
            }
        }
        // no default
    }

    return state;
}

export default CounterReducer;
````

> Envoyer des données supplémentaires avec un dispatch (payload)

`\src\Reducers\AddCartReducer.js`

````javascript
const INITIAL_STATE = {
    cart: 10
}

function AddCartReducer(state = INITIAL_STATE, action) {
  
    switch(action.type){
        case 'ADDCART': {
            return {
                ...state, // copie du state
                cart: action.payload
            }
        }
        // no default
    }

    return state;
}

export default AddCartReducer;
````

`\src\Components\Counter.js`

````javascript
import React, { useState } from 'react';
import { useSelector, useDispatch} from 'react-redux';

export default function Counter() {

    const [cartData, setCartData] = useState(0);
    const cart = useSelector(state => state.cart);
    const dispatch = useDispatch();

    const addToCartFunc = () => {
        dispatch({
            type: "ADDCART",
            payload: cartData
        })
    }

    return (
        <div>
            <h1>Votre panier : { cart }</h1>
            {/* <button onClick={ decrFunc }>-1</button>
            <button onClick={ incrFunc }>+1</button> */}
            <input 
            value={ cartData }
            onInput={ e=> setCartData(e.target.value) }
            type="number"/>
            <br/>
            <button onClick={ addToCartFunc }>Ajouter au panier</button>
        </div>
    )
}
````

`index.js`

````javascript
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import { createStore } from 'redux';
import { Provider } from 'react-redux';
//import CounterReducer from './Reducers/CounterReducer';
import AddCartReducer from './Reducers/AddCartReducer';

const Store = createStore(AddCartReducer);

ReactDOM.render(
  <Provider store={ Store }>
    <App />
  </Provider>,
  document.getElementById('root')
);
````

`App.js`

````javascript
import './App.css';
import Counter from './Components/Counter'

function App() {
  return (
    <div className="App">
      <Counter/>
    </div>
  );
}

export default App;
````

> Combiner plusieurs reducers (combineReducers)

`\src\App.js`

````javascript
import './App.css';
import Counter from './Components/Counter'

function App() {
  return (
    <div className="App">
      <Counter/>
    </div>
  );
}

export default App;
````

`\src\index.js`

````javascript
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import { createStore, combineReducers } from 'redux';
import { Provider } from 'react-redux';
import CounterReducer from './Reducers/CounterReducer';
import AddCartReducer from './Reducers/AddCartReducer';

const rootReducer = combineReducers({
  CounterReducer,
  AddCartReducer
})

const Store = createStore(rootReducer);

ReactDOM.render(
  <Provider store={ Store }>
    <App />
  </Provider>,
  document.getElementById('root')
);
````

`\src\Components\Counter.js`

````javascript
import React, { useState } from 'react';
import { useSelector, useDispatch} from 'react-redux';

export default function Counter() {

    const [cartData, setCartData] = useState(0);
    // const cart = useSelector(state => state.cart);
    const { cart, count } = useSelector(state => ({
        ...state.AddCartReducer,
        ...state.CounterReducer
    }));

    const dispatch = useDispatch();

    const addToCartFunc = () => {
        dispatch({
            type: "ADDCART",
            payload: cartData
        })
    }

    return (
        <div>
            <h1>Votre panier : { cart }</h1>
            {/* <button onClick={ decrFunc }>-1</button>
            <button onClick={ incrFunc }>+1</button> */}
            <input 
            value={ cartData }
            onInput={ e=> setCartData(e.target.value) }
            type="number"/>
            <br/>
            <button onClick={ addToCartFunc }>Ajouter au panier</button>
        </div>
    )
}
````

> Améliorer l'architecture du projet

````text
    .
    ├── src
    │   ├── Components/
    │   ├── redux/
    │       ├── reducers/
    │            ├── addCartReducer.js
    │            ├── counterReducer.js
    │       └── store.js
    .    
    .
````

`\src\App.js`

````javascript
import './App.css';
import Counter from './Components/Counter'

function App() {
  return (
    <div className="App">
      <Counter/>
    </div>
  );
}

export default App;
````

`\src\index.js`

````javascript
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import { Provider } from 'react-redux';
import store from './redux/store';

ReactDOM.render(
  <Provider store={ store }>
    <App />
  </Provider>,
  document.getElementById('root')
);
````

`\src\redux\store.js`

````javascript
import { createStore, combineReducers } from 'redux';
import counterReducer from './Reducers/CounterReducer';
import addCartReducer from './Reducers/AddCartReducer';

const rootReducer = combineReducers({
  counterReducer,
  addCartReducer
})

const store = createStore(rootReducer);

export default store;
````

`\src\redux\reducers\addCartReducer.js`

````javascript
const INITIAL_STATE = {
    cart: 910
}

function AddCartReducer(state = INITIAL_STATE, action) {
  
    switch(action.type){
        case 'ADDCART': {
            return {
                ...state, // copie du state
                cart: action.payload
            }
        }
        // no default
    }

    return state;
}

export default AddCartReducer;
````

`\src\redux\reducers\counterReducer.js`

````javascript
const INITIAL_STATE = {
    count: 100
}

function CounterReducer(state = INITIAL_STATE, action) {
  
    switch(action.type){
        case 'INCR': {
            return {
                ...state, // copie du state
                count: state.count +1
            }
        }
        case 'DECR': {
            return {
                ...state, // copie du state
                count: state.count -1
            }
        }
        // no default
    }

    return state;
}

export default CounterReducer;
````

`\src\Components\Counter.js`

````javascript
import React, { useState } from 'react';
import { useSelector, useDispatch} from 'react-redux';

export default function Counter() {

    const [cartData, setCartData] = useState(0);
    // const cart = useSelector(state => state.cart);
    const { cart, count } = useSelector(state => ({
        ...state.AddCartReducer,
        ...state.CounterReducer
    }));

    const dispatch = useDispatch();

    const addToCartFunc = () => {
        dispatch({
            type: "ADDCART",
            payload: cartData
        })
    }

    return (
        <div>
            <h1>Votre panier : { cart } { count }</h1>
            {/* <button onClick={ decrFunc }>-1</button>
            <button onClick={ incrFunc }>+1</button> */}
            <input 
            value={ cartData }
            onInput={ e=> setCartData(e.target.value) }
            type="number"/>
            <br/>
            <button onClick={ addToCartFunc }>Ajouter au panier</button>
        </div>
    )
}
````

> middleware : les middlewares se trigger (déclenche) lorsque l'on dispatch quelque chose. effectue une action avant le dispatch. Ici on créé une fonction (customMiddleware) que l'on passe à applyMiddleware.

`store.js`

````javascript
import { createStore, combineReducers, applyMiddleware } from 'redux';
import CounterReducer from './Reducers/CounterReducer';
import AddCartReducer from './Reducers/AddCartReducer';

const rootReducer = combineReducers({
  CounterReducer,
  AddCartReducer
})

// curryfication !!!
const customMiddleware = store => next => action => {
    console.log(store);
    console.log(store.getState()); // les states du store avant le dispatch
    console.log(next);// execute le dispatch
    console.log(action);// action du dispatch

    next(action); // on execute le dispatch
} 

const store = createStore(rootReducer, applyMiddleware(customMiddleware));

export default store;
````

> Appel asynchrone avec Redux

Installation du middleware `thunk`, grace à ce middleware nous pouvons passer des fonctions au dispatch (ici getCatImg)

````shell script
npm install redux-thunk
````

`store.js`

````javascript
import { createStore, combineReducers, applyMiddleware } from 'redux';
import counterReducer from './reducers/counterReducer';
import addCartReducer from './reducers/addCartReducer';
import dataImgReducer from './reducers/dataImgReducer';
import thunk from 'redux-thunk';

const rootReducer = combineReducers({
  counterReducer,
  addCartReducer,
  dataImgReducer
})

const store = createStore(rootReducer, applyMiddleware(thunk));

export default store;
````

`dataImgReducer.js`

````javascript
const INITIAL_STATE = {
    imgURL: ""
}

function dataImgReducer(state = INITIAL_STATE, action) {
  
    switch(action.type){
        case 'LOADIMG': {
            return {
                ...state, // copie du state
                imgURL: action.payload
            }
        }
    }

    return state;
}

export default dataImgReducer;

export const getCatImg = () => dispatch => {

    fetch('https://api.thecatapi.com/v1/images/search')
    .then(response => response.json())
    .then(data => {
        dispatch({
            type: 'LOADIMG',
            payload: data[0].url
        })
    })
}
````

`Counter.js`

````javascript
import React, { useState, useEffect } from 'react';
import { useSelector, useDispatch} from 'react-redux';
import { getCatImg } from '../redux/reducers/dataImgReducer';

export default function Counter() {

    const [cartData, setCartData] = useState(0);
    // const cart = useSelector(state => state.cart);
    const { cart, count, imgURL } = useSelector(state => ({
        ...state.AddCartReducer,
        ...state.CounterReducer,
        ...state.dataImgReducer
    }));

    const dispatch = useDispatch();

    const addToCartFunc = () => {
        dispatch({
            type: "ADDCART",
            payload: cartData
        })
    }

    useEffect(() => {
        dispatch(getCatImg())
    }, [])

    return (
        <div>
            <h1>Votre panier : { cart } { count }</h1>
            {/* <button onClick={ decrFunc }>-1</button>
            <button onClick={ incrFunc }>+1</button> */}
            <input 
            value={ cartData }
            onInput={ e=> setCartData(e.target.value) }
            type="number"/>
            <br/>
            <button onClick={ addToCartFunc }>Ajouter au panier</button>
            
            { imgURL && <img style={{ width: "300px" }} src={ imgURL }/>}
        </div>
    )
}
````

---

## Annexes

### Il existe deux façons courantes de configurer une nouvelle application React

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

### Manière intéressante d'insérer du style css dans le jsx

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

 ---

### Passer des props à une route

````javascript
import React, { useState, useEffect } from 'react';
import { useHistory } from 'react-router-dom';
import './choose_game.css';

const Choose_Game = () => {
    const props = 'toto';
    const history = useHistory(props);

    const handleSubmitOnePlayer = (e) => {
        e.preventDefault();
        console.log("in handleSubmitOnePlayer");
        history.push("/game", props);
        // props.history.push("/game");
    }
````

````javascript
import React from 'react'
import { useLocation } from "react-router-dom";
import './game.css'

const Game = (props) => {
  
  console.log(props.location.state); // toto

    return (
````

---

## Librairie `uuid`

````shell script
npm install uuid
````

````javascript
import { v4 as uuidv4 } from 'uuid';
uuidv4(); // ⇨ '9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d'

// or

const { v4: uuidv4 } = require('uuid');
uuidv4(); // ⇨ '1b9d6bcd-bbfd-4b2d-9b5d-ab8dfbbd4bed'
````
