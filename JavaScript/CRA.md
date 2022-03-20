# Create React App

## Alias Webpack

-<https://github.com/Wavelop/cra-with-module-alias/pull/1>

-<https://www.joshwcomeau.com/react/file-structure/?utm_campaign=React%20Hebdo&utm_medium=email&utm_source=Revue%20newsletter>

-<https://github.com/Wavelop/cra-with-module-alias/pull/1>

-<https://github.com/facebook/create-react-app/blob/main/packages/react-scripts/config/webpack.config.js>

### Webpack config

Mise à jour du fichier de config webpack `\node_modules\react-scripts\config\webpack.config.js`

```javascript
        }),
        ...(modules.webpackAliases || {}),
        'Api': path.resolve(__dirname, '../src/api/'),
        'Assets': path.resolve(__dirname, '../src/assets/'),
        'Components': path.resolve(__dirname, '../src/components/'),
        'Contexts': path.resolve(__dirname, '../src/contexts/'),
        'Data': path.resolve(__dirname, '../src/data/'),
        'Hooks': path.resolve(__dirname, '../src/hooks/'),
        'Pages': path.resolve(__dirname, '../src/pages/'),
        'Sass': path.resolve(__dirname, '../src/sass/'),
        'Utils': path.resolve(__dirname, '../src/utils/'),
      },
      plugins: [
```

```javascript
// Avant
import SignInModalsContextProvider from '../../../contexts/SignInModalsContext.jsx'
// Après
import SignInModalsContextProvider from '@Contexts/SignInModalsContext.jsx'
// Après avec la redirection dans un fichier index (voir plus bas)
import SignInModalsContextProvider from '@Contexts'
```

### VS Code

Pour que VS Code reconnaisse nos alias ajouter un fichier `jsconfig.json` à la racine du projet

```JSON
{
  "exclude" : [ "./node_modules" ] , 
  "optionscompilateur" : { 
    "baseUrl" : "." , 
    "chemins" : { 
      "@components/*" : [ "./src/components/*" ] , 
      "@constantes" : [ "./src/constantes/index.js" ] , 
      "@helpers/*" : [ "./src/helpers/*" ] , 
      "@hooks/*" : [ "./src/hooks/*" ] , 
      "@utils" : [ "./src/utils/index.js" ] 
    }
  }
}
```

## redirection dans un fichier : `index.js`

```javascript
// Avant 
import Footer from './components/Footer/Footer.jsx'
// Après
import Footer from './components/Footer'
```

```javascript
// index.js
export * from './Footer'
export {default} from './Footer'
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

### Pourquoi ça marche ?

Etant donné que `Footer/` est un répertoire et que nous essayons de l'importer, le bundler (Webpack) recherchera un fichier `index` (`index.js`, `index.ts` ...), et le fichier `index` fait une redirection vers : `src/components/Footer/Footer.jsx`
