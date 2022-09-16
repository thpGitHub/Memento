# Mock Service Worker (MSW)

Intercepte les requêtes réseau, utile pour les tests, le développement et le débogage.

```shell script
npm install msw --save-dev
```

Bonne pratique commencer par :

- créér un répertoire `src/mocks`
- créér un fichier `src/mocks/handlers.js` qui sera le gestionnaire de toutes les requêtes

MSW peut mocker une `REST API` ou une `GRAPHQL API`

## Mock d'une REST API

```javascript
// src/mocks/handlers.js
import { rest } from 'msw'
```

Les gestionnaires de requêtes contiennent un tableau de requêtes. Pour gérer une requête REST API il faut la méthode (POST, GET ...), le chemin et une fonction qui renvoie la réponse mock.

```javascript
// src/mocks/handlers.js
import { rest } from 'msw'

export const handlers = [
  // Handles a POST /login request
  rest.post('/login', null),
  
  // Handles a GET /user request
  rest.get('/user', null),
]
```

La fonction de réponse possède 3 arguments :

- `req`, une information sur une requête correspondante ;
- `res`, un utilitaire fonctionnel pour créer la réponse simulée ;
- `ctx`, un groupe de fonctions qui aident à définir un code d'état, des en-têtes, un body, un delai, etc. de la réponse simulée.

```javascript
// src/mocks/handlers.js
import { rest } from 'msw'
export const handlers = [
  rest.post('/login', (req, res, ctx) => {
    // Persist user's authentication in the session
    sessionStorage.setItem('is-authenticated', 'true')
    return res(
      // Respond with a 200 status code
      ctx.status(200),
    )
  }),
  rest.get('/user', (req, res, ctx) => {
    // Check if the user is authenticated in this session
    const isAuthenticated = sessionStorage.getItem('is-authenticated')
    if (!isAuthenticated) {
      // If not authenticated, respond with a 403 error
      return res(
        ctx.status(403),
        ctx.json({
          errorMessage: 'Not authorized',
        }),
      )
    }
    // If authenticated, return a mocked user details
    return res(
      ctx.status(200),
      ctx.json({
        username: 'admin',
      }),
    )
  }),
]
```

## Il est possible d'exécuter les mocks dans le navigateur ou dans Node

### Dans le navigateur

- il faut initialiser MSW

```shell script
npx msw init <PUBLIC_DIR> --save
#Exemple avec Create React App
npx msw init public/ --save
```

Cela va  créer le fichier `public\mockServiceWorker.js` et ajouter au `package.json` les lignes suivantes :

```json
"msw": {
    "workerDirectory": "public"
  }
```

- créer un fichier `src/mocks/browser.js`

```javascript
// src/mocks/browser.js
import { setupWorker } from 'msw'
import { handlers } from './handlers'
// This configures a Service Worker with the given request handlers.
export const worker = setupWorker(...handlers)
```

- démarrer le `worker`

⚠ Il n'est pas recommandé d'utiliser MSW en production ! il faut importez le fichier `src/mocks/browser.js` de manière conditionnelle :

```javascript
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom'
import App from './App'

if (process.env.NODE_ENV === 'development') {
  const { worker } = require('./mocks/browser')
  worker.start()
}
ReactDOM.render(<App />, document.getElementById('root'))
```

`worker.start()` est une promise il est donc possible de retarder le rendu :

```javascript
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom'
import App from './App'

function prepare() {
  if (process.env.NODE_ENV === 'development') {
    const { worker } = require('./mocks/browser')
    return worker.start()
  }
  return Promise.resolve()
}

prepare().then(() => {
  ReactDOM.render(<App />, document.getElementById('root'))
})
```

#### Exemple simple d'une structure du dossier `mocks/`

````text
    .
    ├── browser.js
    ├── handlers.js
    └── index.js
````

```javascript
// index.js
if (process.env.NODE_ENV === 'development') {
    module.exports = require('./browser')
}
```

```javascript
// browser.js
import { setupWorker } from 'msw'
import { handlers } from './handlers'

export const worker = setupWorker(...handlers)

worker.start()
```

```javascript
// handlers.js
import {rest} from 'msw'

export const handlers = [
    // Handles a POST /login request
    rest.post('/login', null),

    // Handles a GET /user request
    rest.get('https://example.com/api/login', (req, res, ctx) => {
        return res(
            // ctx.delay(1500),
            ctx.status(202, 'Mocked status'),
            ctx.json({
                message: 'Mocked response JSON body',
            }),
        )
    }),
]
```

```javascript
// app.js
import * as React from 'react'
import './mocks'
```

### Dans Node

- créer un fichier `src/mocks/server.js`

```javascript
// src/mocks/server.js
import { setupServer } from 'msw/node'
import { handlers } from './handlers'
// This configures a request mocking server with the given request handlers.
export const server = setupServer(...handlers)
```

## `json()`

Transforme le `body` de la réponse en JSON et ajoute `Content-Type: application/json` au `header` de la réponse.

Signature

````javascript
function json(data: Record<string, any>): MockedResponse
````

Exemple

````javascript
rest.get('/user', (req, res, ctx) => {
  return res(
    ctx.json({
      firstName: 'John',
      age: 32,
    }),
  )
})
````
