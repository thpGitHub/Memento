# import

## Optimisation avec `Create React App`

```javascript
// Avant
import {SignInModalsContext} from '../../../contexts/SignInModalsContext.jsx'
// Après
import {SignInModalsContext} from 'contexts/SignInModalsContext'
```

Pour mettre des chemins absolus dans les imports il faut créer à la racine du projet un fichier `jsconfig.json` ou `tsconfig.json`

```javascript
//jsconfig.json
{
  "compilerOptions": {
    "baseUrl": "src"
  },
  "include": ["src"]
}
```

## Optimisation avec un fichier `index.js`

```javascript
// Avant 
import Footer from './components/Footer/Footer.jsx'
// Après
import Footer from './components/Footer'
```

```text
    .
    ├── src
        ├── Components/
            ├── Footer/
                ├── Footer.jsx
                ├── Footer.scss
                ├── Footer.test.js
                ├── index.js
```

```javascript
// index.js
export * from './Footer'
export {default} from './Footer'
```

### Pourquoi ça marche ?

Etant donné que `Footer/` est un répertoire et que nous essayons de l'importer, le bundler (Webpack) recherchera un fichier `index` (`index.js`, `index.ts` ...), et le fichier `index` fait une redirection vers : `src/components/Footer/Footer.jsx`

## Les deux optimisations ensemble

```javascript
import Footer from 'components/Footer'
```
