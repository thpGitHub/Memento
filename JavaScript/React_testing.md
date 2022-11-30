# React Testing

1. [Jest](#jest)
1. [React Testing Library](#rtl)
1. [React Testing Library/jest-dom](#rtl/jd)
1. [Tester des formulaires](#form)
1. [Mocker les requêtes HTTP avec `MSW`](#msw)
1. [Mocker l'API du navigateur](#api)
1. [Mocker un module](#module)
1. [Test componsant avec Jest sans React Testing Library](#onlyjest)
1. [Test componsant avec Jest ET React Testing Library](#jestrtl)
1. [Test componsant avec Jest ET React Testing Library et l'extension `jest-dom`](#jestrtldom)
1. [Test en `boite noire`](#boite)
1. [`userEvent` de testing library](#userevent)
1. [Annexes](#annexes)

## `Jest` <a name="jest"></a>

Avec `create-react-app` `Jest` est déjà installé.

`Jest` recherchera des fichiers de test avec l'une des conventions de dénomination courantes suivantes :

- Fichiers avec `.js` suffixe dans le dossier `__tests__`.
- Fichiers avec `.test.js` suffixe.
- Fichiers avec `.spec.js` suffixe.

Les fichiers `.test.js/` `.spec.js`(ou les dossiers `__tests__`) peuvent être situés à n'importe quelle profondeur dans le dossier `src`.

Nous vous recommandons de placer les fichiers (ou dossiers `__tests__`) de test à côté du code qu'ils testent afin que les importations relatives apparaissent plus courtes. Par exemple, si `App.test.js` et `App.js` se trouvent dans le même dossier, le test n'a besoin que de `import App from './App'` au lieu d'un long chemin relatif. La colocalisation permet également de trouver des tests plus rapidement dans des projets plus importants.

### les principaux types de tests sont

- Les `tests unitaires` vont venir tester une petite partie de votre code de manière totalement indépendante : une fonction, un bout de script... Ils sont les plus rapides à écrire, mais n'assurent pas forcément nos arrières.

- Les `tests end-to-end`, quant à eux, permettent de tester l'intégralité d'une fonctionnalité de bout en bout. Ils sont beaucoup plus sécurisants, mais prennent donc beaucoup de temps à écrire.

- Les `tests d'intégration` qui sont souvent considérés comme le juste milieu entre sécurité fournie et temps requis pour les rédiger. Ils permettent de tester une fonctionnalité, en simulant des interactions utilisateur pour s'assurer que tout fonctionne bien comme prévu.

### Exemple de `test unitaire` avec `Jest`

```javascript
// multiplication.js
function multiplication(a, b) {
    return a*b;
}
export default multiplication;
```

```javascript
// multiplication.test.js
import multiplication from './multiplication'

test('multiplication de number', () => {
    expect(multiplication(2,3)).toBe(6);
    expect(typeof multiplication(2,3)).toBe('number');
    expect(multiplication(2,3)).toEqual(6);
    expect(multiplication('toto','tata')).toEqual(Error('Number expected as parameter'));
    expect(multiplication(6,'tata')).toEqual(Error('Number expected as parameter'));
    expect(multiplication('tata', 6)).toEqual(Error('Number expected as parameter'));
})
```

`toBe` et `toEqual` sont des `matchers` fournis par `Jest`.
La fonction `expect()` compare un élément avec les `matchers`

Par convention on remplace `test()` par `it()` si on commence nos tests par `should`

````javascript
import multiplication from './multiplication'

it('multiplication de number', () => {
    expect(multiplication(2,3)).toBe(6);
    expect(typeof multiplication(2,3)).toBe('number');
    expect(multiplication(2,3)).toEqual(6);
    expect(multiplication('toto','tata')).toEqual(Error('Number expected as parameter'));
    expect(multiplication(6,'tata')).toEqual(Error('Number expected as parameter'));
    expect(multiplication('tata', 6)).toEqual(Error('Number expected as parameter'));
})
````

Pour mesurer la couverture de tests (code coverage) :

````shell script
npm test -- --coverage
````

### Jest met à disposition des hooks pour la préparation et le nettoyage

````javascript
// sera appelé avant chaque test
beforeEach(() => {
});

// sera appelé après chaque test
afterEach(() => {
});

// sera appelé une seul fois avant tous les tests
beforeAll(() => {
});

// sera appelé une seul fois après tous les tests
afterAll(() => {
});

````

## React Testing Library <a name="rtl">

Avec `create-react-app` `React Testing Library` est déjà installé.

Est une bibliothèque qui donne accès à davantage d'outils permettant de tester des composants. Pour tester nos composants, il faudra donc faire un `render`, vérifier le DOM généré, et le comparer avec ce qui était attendu. Cette bibliothèque ne remplace pas Jest, au contraire, elle est complémentaire à Jest. `React Test Library` nous permet de nous concentrer sur le DOM, en le recréant, en permettant de simuler des interactions et de vérifier ce qui est rendu. Cela nous aide à nous mettre dans la peau de nos utilisateurs, et à anticiper ce qu'ils verront.

Les `Queries` sont les méthodes que Testing Library vous propose pour rechercher des éléments sur la page. Il existe plusieurs types de requêtes (`get`, `find`, `query`), la différence entre eux est de savoir si la requête générera une erreur si aucun élément n'est trouvé ou si elle renverra une promesse et réessayera.

`Jest` est l'outil de base pour nos tests, et `React Testing Library` est l'outil qui nous facilite les tests de composants.

pour info :

```shell script
# installation de @testing-library/react et @testing-library/jest-dom
npm install --save @testing-library/react @testing-library/jest-dom
```

Pour éviter d'`import '@testing-library/jest-dom';` à chaque fois dans les fichiers de test il faut créer un fichier : `src/setupTests.js` contenant :

```javascript
// react-testing-library renders your components to document.body,
// this adds jest-dom's custom assertions
import '@testing-library/jest-dom';
```

- <https://github.com/testing-library/react-testing-library>

```javascript
import Hello from '../../components/hello'
import {render, fireEvent} from '@testing-library/react'

test('Affiche "Bonjour John" et "Merci" lors d\'un click" ', () => {
  const {container} = render(<Hello name="John" />)
  const envoyer = container.querySelector('input')
  const label = container.firstChild.querySelector('div')

  expect(label.textContent).toBe(`Bonjour John`)
  fireEvent.click(envoyer)
  expect(label.textContent).toBe(`Merci`)
})
```

`userEvent` vs `fireEvent` A voir !

```javascript
import MenuItem from "./MenuItem";
import {render, screen} from '@testing-library/react' 
import userEvent from '@testing-library/user-event'

const item = {    
          "id": "1519055545-88",
          "title": "Brunch authentique 1 personne",
          "description": "Assiette de jambon cuit",
          "price": "25.00",
          "picture": "https://f.roocdn.com/images/menu_items/1583350/item-image.jpg",
          "popular": true,      
}
// mock function
const onClick = jest.fn()

test('should render without error', ()=> {
    const renderMenuItem = () => {
        render(<MenuItem item={item} onClick={onClick()}/>)
    }
    expect(renderMenuItem).not.toThrow()
})

test('should to have display h3', ()=> {
    render(<MenuItem item={item} onClick={onClick}/>)
    
    expect(onClick).not.toHaveBeenCalled()

    const titleElem = screen.getByText('Brunch authentique 1 personne')
    expect(titleElem).toBeInTheDocument()
    // or same
    const headingH3 = screen.getByRole('heading', {level: 3})
    expect(headingH3).toHaveTextContent('Brunch authentique 1 personne')

    userEvent.click(headingH3)
    expect(onClick).toHaveBeenCalled()
})

test('should img with its alt', ()=> {
    render(<MenuItem item={item} onClick={onClick}/>)

    // <img src={item.picture} alt={item.title}
    const imgElem = screen.getByAltText('Brunch authentique 1 personne')
    expect(imgElem).toBeInTheDocument()
})

test('should not display image if picture is not defined', () => {
    const itemWithPictureNotDefined = {    
        "id": "1519055545-88",
        "title": "Brunch authentique 1 personne",
        "description": "Assiette de jambon cuit",
        "price": "25.00",
        // "picture": "https://f.roocdn.com/images/menu_items/1583350/item-image.jpg",
        "popular": true,      
}
    render(<MenuItem item={itemWithPictureNotDefined} onClick={onClick}/>)
    
    const imgElem = screen.queryByAltText('Brunch authentique 1 personne')
    expect(imgElem).toBeNull()
})
```

## testing-library/jest-dom <a name="rtl/jd">

`Matchers` Jest personnalisés pour tester l'état du DOM.
Cette librairie propose des assertions supplémentaires aux assertions de Jest (qui sont plus génériques). Par exemple si nous voulons savoir si un élément contient un classe css nous pourrions utiliser :

```javascript
expect(deleteButton).toHaveClass('btn-link')
```

- <https://github.com/testing-library/jest-dom>

Exemple :

```javascript
import Footer from './Footer.jsx'
import { render } from '@testing-library/react'
 
describe('Footer', () => {
    test('Should render without crash', async () => {
        render(<Footer />)
    })
})
```

Exemple :

```javascript
import Hello from '../../components/hello'
import {render, fireEvent} from '@testing-library/react'

test('Affiche "Bonjour John" et "Merci" lors d\'un click" ', () => {
  const {container} = render(<Hello name="John" />)
  const envoyer = container.querySelector('input')
  const label = container.firstChild.querySelector('div')

  expect(label).toHaveTextContent(`Bonjour John`)
  fireEvent.click(envoyer)
  expect(label).toHaveTextContent(`Merci`)
})
```

Exemple :

`Search.js`

```javascript
import {useState, useContext} from 'react'
import './Search.css'
//** react icons **/
import {IconContext} from 'react-icons'
import {MdImageSearch} from 'react-icons/md'
//** Contexts **/
import {ThemeContext} from '../Contexts/ThemeContext.js'
// ** img **
import {ReactComponent as SunLight} from '../Assets/sun-color.svg'
import {ReactComponent as SunDark} from '../Assets/sun-warm.svg'

export default function Search({onChangeQuery, stateFetchPhotos}) {
  const [searchInput, setSearchInput] = useState('')

  const {toggleTheme, theme} = useContext(ThemeContext)

  const handleChange = e => {
    e.preventDefault()
    setSearchInput(e.target.value)
    // console.log('status in search component ===', stateFetchPhotos)
  }

  const handleChangeQuery = e => {
    e.preventDefault()
    if (searchInput !== '') {
      onChangeQuery(searchInput)
      setSearchInput('')
    }
  }

  return (
    <form className="search-container" onSubmit={handleChangeQuery}>
      <input
        type="search"
        className="search-input"
        placeholder=" Search Photos ...."
        onChange={handleChange}
        value={searchInput}
        autoFocus
      />
      <IconContext.Provider value={{className: 'react-icons-search'}}>
        <button type="submit" className="search-button">
          <MdImageSearch />
        </button>
        <button
          onClick={toggleTheme}
          className={theme ? 'btn-toggle' : 'btn-toggle dark-btn'}
        >
          {theme ? (
            <SunLight data-testid="SunLight" />
          ) : (
            <SunDark data-testid="SunDark" />
          )}
        </button>
      </IconContext.Provider>
      {stateFetchPhotos.status === 'loading' && (
        <span data-testid="Loading">⏳ Loading...</span>
      )}
      {stateFetchPhotos.status === 'fail' && (
        <span data-testid="Fail">❌ {stateFetchPhotos.fail.message}</span>
      )}
    </form>
  )
}
```

`Search.test.js`

```javascript
import Search from '../Components/Search.js'
import {render, fireEvent, screen} from '@testing-library/react'
//** Contexts **/
import ThemeContextProvider from '../Contexts/ThemeContext.js'

describe('Search Component', () => {
  const SearchComponent = (statePhotos) => {
    const handleChangeQuery = changeQuery => {
      const fakeChangeQuery = changeQuery
    }
    const stateFetchPhotos =  {
      status: statePhotos,
      fail: {
        Message: 'error'
      }
    }
    render(
      <ThemeContextProvider>
        <Search
          onChangeQuery={handleChangeQuery}
          stateFetchPhotos={stateFetchPhotos}
        />
      </ThemeContextProvider>,
    )
  }

  it('Should render without crashing', async () => {
    SearchComponent()
  })

  it('Should icone theme SunDark exist', async () => {
    SearchComponent()
    expect(screen.getByTestId('SunDark')).toBeTruthy()
  })

  it('Should change theme', async () => {
    SearchComponent()
    let element = screen.getByTestId('SunDark')

    fireEvent.click(element)

    element = screen.getByTestId('SunLight')
    expect(element.getAttribute('data-testid')).toBe('SunLight')
  })
  it('Should display ⏳ Loading...', async () => {
    SearchComponent('loading')
    
    const element = screen.getByTestId('Loading')
    expect(element.textContent).toBe('⏳ Loading...')
  })
  it('Should display ❌', async () => {
    SearchComponent('fail')
    
    const element = screen.getByTestId('Fail')
    expect(element.textContent).toMatch(/❌/)
  })
})
```

## Tester des formulaires <a name="form">

```javascript
/**
     * find the input with 'textbox' (textbox = is label of input)
     */ 
    const firsNameElement = screen.getByRole('textbox', {name: /\* Prénom/i} )
    const nameElement = screen.getByRole('textbox', {name: /\* Nom de famille/i} )
    const emailElement = screen.getByRole('textbox', {name: /\* E-mail/i} )
    /**
     * https://github.com/testing-library/dom-testing-library/issues/567
     * for input type password use : getByLabelText
     */
    const pswElement = screen.getByLabelText(/\* Mot de passe/i)
    const repeatPswElement = screen.getByLabelText(/\* Répéter mot de passe/i)

    const submitedButtonElement = screen.getByRole('button', {name: /Soumettre/i})

    fireEvent.change(firsNameElement, {target: {firsName}})
    fireEvent.change(nameElement, {target: {name}})
    fireEvent.change(emailElement, {target: {email}})
    fireEvent.change(pswElement, {target: {psw}})
    fireEvent.change(repeatPswElement, {target: {repeatPsw}})

    fireEvent.click(submitedButtonElement)
```

Exemple

````javascript
//LoginForm
import * as React from 'react'

function LoginForm({onSubmit}) {
  const [username, setUsername] = React.useState('')
  const [password, setPassword] = React.useState('')
  const handleSubmit = e => {
    e.preventDefault()
    console.log(`handleSubmit avec ${username}, ${password}`)
    onSubmit({username, password})
  }
  return (
    <form onSubmit={handleSubmit}>
      <label htmlFor="username">Nom d'utilisateur :</label>
      <input
        id="username"
        type="text"
        value={username}
        onChange={e => setUsername(e.target.value)}
      />

      <label htmlFor="password">Mot de passe :</label>
      <input
        id="password"
        type="text"
        value={password}
        onChange={e => setPassword(e.target.value)}
      />

      <div>
        <input type="submit" value={'Connexion'} />
      </div>
    </form>
  )
}

export default LoginForm
````

````javascript
import * as React from 'react'
import LoginForm from '../../components/loginForm'
import {render, screen, fireEvent} from '@testing-library/react'

test('formulaire de login avec username et password" ', () => {
  let submittedUsername
  let submittedPassword

  const handleSubmit = ({username, password}) => {
    submittedUsername = username
    submittedPassword = password
  }

  render(<LoginForm onSubmit={handleSubmit} />)
  const username = 'toto'
  const password = 'toto2'

  const usernameElement = screen.getByRole('textbox', {
    name: /Nom d'utilisateur :/i,
  })
  const passwordElement = screen.getByRole('textbox', {
    name: /Mot de passe :/i,
  })
  const submitButtonElement = screen.getByRole('button', {
    name: /Connexion/i,
  })
  // modification de la valeur des 2 champs input avec fireEvent.change
  fireEvent.change(usernameElement, {target: { value: username }});
  fireEvent.change(passwordElement, {target: { value: password }});
  //simulation d'un click sur le button connexion
  fireEvent.click(submitButtonElement)

  expect(submittedUsername).toBe(username)
  expect(submittedPassword).toBe(password)
})
````

Même chose en remplacant `fireEvent` vs `userEvent` et `getByRole` vs `getByText`

````javascript
  const usernameElement = screen.getByText(/Nom d'utilisateur :/i)
  const passwordElement = screen.getByText(/Mot de passe :/i)
  const submitButtonElement = screen.getByRole('button', {
    name: /Connexion/i,
  })
  userEvent.type(usernameElement, username)
  userEvent.type(passwordElement, password)
  userEvent.click(submitButtonElement)

  expect(submittedUsername).toBe(username)
  expect(submittedPassword).toBe(password)
````

Même chose en mockant la fonction `handleSubmit` que l'on ne souhaite plus implémenter pour nos tests

````javascript
import * as React from 'react'
import LoginForm from '../../components/loginForm'
import {render, screen} from '@testing-library/react'
import userEvent from '@testing-library/user-event'

test('formulaire de login avec username et password" ', () => {
  const handleSubmit = jest.fn()

  render(<LoginForm onSubmit={handleSubmit} />)
  const username = 'toto'
  const password = 'toto2'

  const usernameElement = screen.getByText(/Nom d'utilisateur :/i)
  const passwordElement = screen.getByText(/Mot de passe :/i)
  const submitButtonElement = screen.getByRole('button', {
    name: /Connexion/i,
  })
  userEvent.type(usernameElement, username)
  userEvent.type(passwordElement, password)
  userEvent.click(submitButtonElement)
  expect(handleSubmit).toHaveBeenCalled()
  expect(handleSubmit).toHaveBeenCalledWith({username, password})
  expect(handleSubmit).toHaveBeenCalledTimes(1)
})
````

Même chose en utilisant la librairie `faker`

````javascript
import * as React from 'react'
import LoginForm from '../../components/loginForm'
import {render, screen} from '@testing-library/react'
import userEvent from '@testing-library/user-event'
import faker from 'faker'

test('formulaire de login avec username et password" ', () => {
  const handleSubmit = jest.fn()

  render(<LoginForm onSubmit={handleSubmit} />)
  const username = faker.internet.userName()
  const password = faker.internet.password()

  const usernameElement = screen.getByText(/Nom d'utilisateur :/i)
  const passwordElement = screen.getByText(/Mot de passe :/i)
  const submitButtonElement = screen.getByRole('button', {
    name: /Connexion/i,
  })
  userEvent.type(usernameElement, username)
  userEvent.type(passwordElement, password)
  userEvent.click(submitButtonElement)

  expect(handleSubmit).toHaveBeenCalled()
  expect(handleSubmit).toHaveBeenCalledWith({username, password})
  expect(handleSubmit).toHaveBeenCalledTimes(1)
})
````

## Mocker les requêtes HTTP avec `MSW` <a name="msw">

Mock Service Worker (MSW) est une bibliothèque de simulation d'API pour le navigateur et Node.js et utilise l'API Service Worker pour intercepter les demandes réelles.

```shell script
npm install --save-dev msw
```

````javascript
// LoginSubmit
import * as React from 'react'
import LoginForm from './loginForm'

function LoginSubmit() {
  const [dataToSubmit, setDataToSubmit] = React.useState(null)
  const [fetchStatus, setFetchStatus] = React.useState({
    status: 'idle',
    payload: '',
  })

  const endpoint = 'https://example.com/api/login'
  const body = dataToSubmit ? JSON.stringify(dataToSubmit) : null
  React.useEffect(() => {
    if (body) {
      setFetchStatus({status: 'feching', payload: ''})
      fetch(endpoint, {
        method: 'POST',
        body: body,
        headers: {
          'content-type': 'application/json',
        },
      }).then(async response => {
        const data = await response.json()
        if (response.ok) {
          setFetchStatus({status: 'done', payload: data})
        } else {
          setFetchStatus({status: 'error', payload: data})
        }
      })
    }
  }, [dataToSubmit, body])
  return (
    <div>
      {fetchStatus.status === 'feching' ? 'chargement...' : null}

      {fetchStatus.status === 'error' ? (
        <div role="alert" style={{color: 'red'}}>
          {fetchStatus.payload.errorMessage}
        </div>
      ) : null}
      {fetchStatus.status === 'done' ? (
        <div>
          Bonjour <strong>{fetchStatus.payload.username}</strong>
        </div>
      ) : (
        <LoginForm onSubmit={formData => setDataToSubmit(formData)} />
      )}
    </div>
  )
}

export default LoginSubmit
````

````javascript
//LoginSubmit.test
import * as React from 'react'
import LoginSubmit from '../../components/loginSubmit'
import {render, screen, waitFor} from '@testing-library/react'
import userEvent from '@testing-library/user-event'
import faker from 'faker'
import {rest} from 'msw'
import {setupServer} from 'msw/node'

const sleep = t => new Promise(resolve => setTimeout(resolve, t))
const DELAY = 100

const server = setupServer(
  rest.post('https://example.com/api/login', (req, res, ctx) => {
    if (!req.body.password) {
      return res(
        ctx.status(400),
        ctx.json({errorMessage: 'le password est obligatoire !'}),
        ctx.delay(DELAY)
      )
    }
    if (!req.body.username) {
      return res(
        ctx.status(400),
        ctx.json({errorMessage: 'username est obligatoire !'}),
        ctx.delay(DELAY)
      )
    }
    return res(ctx.delay(DELAY), ctx.json({username: req.body.username}))
  }),
)

beforeAll(() => server.listen())
afterAll(() => server.close())

test('login api affiche le nom de l\'utilisateur connecté" ', async () => {
  render(<LoginSubmit />)

  const username = faker.internet.userName()
  const password = faker.internet.password()

  const usernameElement = screen.getByText(/Nom d'utilisateur :/i)
  const passwordElement = screen.getByText(/Mot de passe :/i)
  const submitbuttonElement = screen.getByRole('button', {name: /Connexion/i})

  userEvent.type(usernameElement, username)
  userEvent.type(passwordElement, password)
  userEvent.click(submitbuttonElement)

  await waitFor(() => sleep(150))
  expect(screen.getByText(username)).toBeInTheDocument()
})
````

`waitFor` c'est bien mais il faut calculer le temps à attendre (150 car il y avait un delay de 100) on peut utiliser à la place `waitForElementToBeRemoved`

````javascript
test('login api affiche le nom de l\'utilisateur connecté" ', async () => {
  render(<LoginSubmit />)

  const username = faker.internet.userName()
  const password = faker.internet.password()

  const usernameElement = screen.getByText(/Nom d'utilisateur :/i)
  const passwordElement = screen.getByText(/Mot de passe :/i)
  const submitbuttonElement = screen.getByRole('button', {name: /Connexion/i})

  userEvent.type(usernameElement, username)
  userEvent.type(passwordElement, password)
  userEvent.click(submitbuttonElement)

  await waitForElementToBeRemoved(() => screen.getByText(/chargement.../))
  expect(screen.getByText(username)).toBeInTheDocument()
})
````

Gérer les erreurs

````javascript
test('login api erreur : manque password" ', async () => {
  render(<LoginSubmit />)

  const username = faker.internet.userName()
  const password = faker.internet.password()

  const usernameElement = screen.getByText(/Nom d'utilisateur :/i)
  const passwordElement = screen.getByText(/Mot de passe :/i)
  const submitbuttonElement = screen.getByRole('button', {name: /Connexion/i})

  userEvent.type(usernameElement, username)
  // userEvent.type(passwordElement, password)
  userEvent.click(submitbuttonElement)

  await waitFor(() => sleep(150))
  expect(screen.getByRole('alert')).toHaveTextContent('le password est obligatoire !')
})
````

Gérer les erreurs avec `Snapshot` de `Jest` afin de regénérer le code des messages (ici d'erreur). `toMatchInlineSnapshot`

````javascript
test('login api erreur : manque password" ', async () => {
  render(<LoginSubmit />)

  const username = faker.internet.userName()
  const password = faker.internet.password()

  const usernameElement = screen.getByText(/Nom d'utilisateur :/i)
  const passwordElement = screen.getByText(/Mot de passe :/i)
  const submitbuttonElement = screen.getByRole('button', {name: /Connexion/i})

  userEvent.type(usernameElement, username)
  // userEvent.type(passwordElement, password)
  userEvent.click(submitbuttonElement)

  await waitFor(() => sleep(150))
  expect(screen.getByRole('alert').textContent).toMatchInlineSnapshot()
  
  // Après l'exécution du test le code sera automatiquement écrit avec
  expect(screen.getByRole('alert').textContent).toMatchInlineSnapshot(
    `"le password est obligatoire !"`,
  )

  // Si le message change il faudra mettre à jour le Snapshot soit manuellement dans le test soit dans le terminal.

  // message erreur dans terminal
   Snapshot: "le password est obligatoire !"
   Received: "le password est obligatoire !!"

   // dans le terminal :
    › Press i to update failing snapshots interactively.
})
````

Gerer erreur serveur : exemple 503

````javascript
// afterEach(() => server.resetHandlers)
// server.use() 
import * as React from 'react'
import LoginSubmit from '../../components/loginSubmit'
import {
  render,
  screen,
  waitForElementToBeRemoved,
  waitFor,
} from '@testing-library/react'
import userEvent from '@testing-library/user-event'
import faker from 'faker'
import {rest} from 'msw'
import {setupServer} from 'msw/node'

const sleep = t => new Promise(resolve => setTimeout(resolve, t))
const DELAY = 100

const server = setupServer(
  rest.post('https://example.com/api/login', (req, res, ctx) => {
    if (!req.body.password) {
      return res(
        ctx.status(400),
        ctx.json({errorMessage: 'le password est obligatoire !!'}),
        ctx.delay(DELAY),
      )
    }
    if (!req.body.username) {
      return res(
        ctx.status(400),
        ctx.json({errorMessage: 'username est obligatoire !'}),
        ctx.delay(DELAY),
      )
    }
    // return res(ctx.delay(DELAY), ctx.json({username: req.body.username}))
    return res(ctx.json({username: req.body.username}))
  }),
)

beforeAll(() => server.listen())
afterAll(() => server.close())
afterEach(() => server.resetHandlers)
// .
// .
// .
test('erreur 503', async () => {
  render(<LoginSubmit />)

  server.use(
    rest.post('https://example.com/api/login', (req, res, ctx) => {
      return res(
        ctx.status(503),
        ctx.json({errorMessage: 'service indisponible'}),
      )
    }),
  )

  const username = faker.internet.userName()
  const password = faker.internet.password()

  const usernameElement = screen.getByText(/Nom d'utilisateur :/i)
  const passwordElement = screen.getByText(/Mot de passe :/i)
  const submitbuttonElement = screen.getByRole('button', {name: /Connexion/i})

  userEvent.type(usernameElement, username)
  // userEvent.type(passwordElement, password)
  userEvent.click(submitbuttonElement)

  await waitFor(() => sleep(150))
  expect(screen.getByRole('alert').textContent).toMatchInlineSnapshot(
    `"service indisponible"`,
  )
})
````

Autre exemple

```javascript
import { rest } from 'msw';
import { setupServer } from 'msw/node'
import axios from 'axios';

const server = setupServer(
  // on mock la requete GET https://api.example.com/users/:userId
  rest.get("https://api.example.com/users/:userId", (req, res, ctx) => {
    const { userId } = req.params;
    return res(
      ctx.json({
        id: userId,
        firstName: "John",
        lastName: "Maverick",
      })
    );
  })
);

beforeAll(() => server.listen());

afterEach(() => server.resetHandlers());

afterAll(() => server.close());

test("api should respond", async () => {
  const response = await axios.get("https://api.example.com/users/john");
  expect(response.data).toEqual({
    firstName: "John",
    id: "john",
    lastName: "Maverick",
  });
});
```

Avec plusieurs fichiers

`server.js`

```javascript
import {rest} from 'msw'
import {setupServer} from 'msw/node'

const server = setupServer(
  // on mock la requete GET https://api.example.com/users/:userId
  rest.get(
    'https://lereacteur-deliveroo-api.herokuapp.com',
    (req, res, ctx) => {
      return res(
        ctx.json({
          restaurant: {
            path: 'Le Pain Quotidien',
            name: 'Le Pain Quotidien - Montorgueil',
            categories: ['Petit Déjeuner', 'Salade', 'Brunch', 'Boulangerie'],
            price: '€€',
            phone: '+33144780895',
            percentage: 87,
            ratings: '50+',
            address: '8 Rue de Bretagne, 75003 Paris',
            delay: '10 - 20 Mins (Au plus tôt)',
            description:
              'Profitez de chaque plaisir de la vie quotidienne. Le Pain Quotidien propose des ingrédients simples et sains, du bon pain, des fruits et des légumes frais et de saison issus de l’agriculture biologique.',
            picture: 'https://f.roocdn.com/images/menus/17697/header-image.jpg',
            client_address: {
              coordinates: [2.36051359999999, 48.8737157],
              locality: 'Paris',
              country: 'FR',
              formatted_address: '25 Passage Dubail, 75010 Paris, France',
              post_code: '75010',
              route: 'Passage Dubail',
              street_number: '25',
              city: 'Paris',
            },
          },
          categories: [
            {
              name: 'Brunchs',
              meals: [
                {
                  id: '1519055545-88',
                  title: 'Brunch authentique 1 personne',
                  description:
                    'Assiette de jambon cuit, jambon fumeì, terrine, comté bio & camembert bio, salade jeunes pousses, oeuf poché bio, pain bio & confiture, 1 viennoiserie bio au choix, granola parfait bio, jus de fruits 33cl au choix',
                  price: '25.00',
                  picture:
                    'https://f.roocdn.com/images/menu_items/1583350/item-image.jpg',
                  popular: true,
                },
                {
                  id: '1519055545-89',
                  title: 'Brunch vegan',
                  description:
                    'Falafels bio, houmous bio, avocat aux super graines bio, lentilles au paprika, herbes fraîches, houmous de carotte et légumes de saison, soupe du jour bio, pain bio & confiture, crunola parfait aux fruits de saison, flûte aux raisins et noisettes, jus de fruits 33cl au choix',
                  price: '25.00',
                  picture:
                    'https://f.roocdn.com/images/menu_items/3905693/item-image.jpg',
                },
              ],
            },
            {
              name: 'Petit déjeuner',
              meals: [
                {
                  id: '1519055545-90',
                  title: 'Petit-déjeuner 1 personne',
                  description:
                    'Assortiment de pains bio, beurre & confitures bio + 1 viennoiserie bio au choix + 1 boisson fraîche au choix',
                  price: '10.40',
                },
                {
                  id: '1519055545-91',
                  title: 'Fromage blanc bio au miel',
                  description: '',
                  price: '10.40',
                },
                {
                  id: '1519055545-92',
                  title: 'Granola parfait bio',
                  description: 'Yaourt, granola, et fruits frais de saison',
                  price: '6.60',
                  picture:
                    'https://f.roocdn.com/images/menu_items/1323271/item-image.jpg',
                  popular: true,
                },
                {
                  id: '1519055545-93',
                  'web-scraper-start-url':
                    'https://deliveroo.fr/fr/menu/paris/3eme-temple/le-pain-quotidien-bretagne',
                  title: 'Crunola parfait bio (100% végétalien)',
                  description:
                    '100% végétalien - granola cru, banane, lait de coco et beurre de noix de cajou',
                  price: '6.60',
                },
                {
                  id: '1519055545-137',
                  'web-scraper-start-url':
                    'https://deliveroo.fr/fr/menu/paris/3eme-temple/le-pain-quotidien-bretagne',
                  title: 'Salade de fruits bio de saison',
                  description:
                    'Pomme, ananas, kiwi, orange, grenade, myrtilles',
                  price: '6.90',
                  picture:
                    'https://f.roocdn.com/images/menu_items/2549378/item-image.jpg',
                },
                {
                  id: '1519055545-95',
                  'web-scraper-start-url':
                    'https://deliveroo.fr/fr/menu/paris/3eme-temple/le-pain-quotidien-bretagne',
                  title: 'Omelette au four de saison',
                  description:
                    'Courge butternut, chèvre & thym, avec une salade de jeunes pousses',
                  price: '6.60',
                },
                {
                  id: '1519055545-96',
                  'web-scraper-start-url':
                    'https://deliveroo.fr/fr/menu/paris/3eme-temple/le-pain-quotidien-bretagne',
                  title: 'Chia bowl',
                  description:
                    'Graines de chia bio, myrtilles, grenades, crunola bio',
                  price: '6.60',
                  popular: true,
                },
                {
                  id: '1519055545-97',
                  'web-scraper-start-url':
                    'https://deliveroo.fr/fr/menu/paris/3eme-temple/le-pain-quotidien-bretagne',
                  title: 'Bircher Muesli',
                  description:
                    'Muesli maison bio, boisson à l’amande bio, fruits de saison et super graines bio (VEGAN)',
                  price: '6.60',
                  picture:
                    'https://f.roocdn.com/images/menu_items/5250391/item-image.jpg',
                },
              ],
            },

            {
              name: 'Desserts',
              meals: [],
            },
          ],
        }),
      )
    },
  ),
)

export default server

```

`App.test.js`

```javascript
import server from './tests/utils/server'
import {getByText, render, screen} from '@testing-library/react'
import userEvent from '@testing-library/user-event'
import App from './App'

beforeAll(() => server.listen())

afterEach(() => server.resetHandlers())

afterAll(() => server.close())

test('should display data from API', async () => {
  render(<App />)

  const loaderElem = screen.getByText('En cours de chargement...')
  expect(loaderElem).toBeInTheDocument()

  const titleElem = await screen.findByText('Le Pain Quotidien - Montorgueil')
  expect(titleElem).toBeInTheDocument()
})

test('should add item to card', async () => {
  render(<App />)

  await screen.findByText('Le Pain Quotidien - Montorgueil')

  const emptyCarElem = screen.getByText('Votre panier est vide')
  expect(emptyCarElem).toBeInTheDocument()

  const brunchElem = screen.getByText('Brunch authentique 1 personne')
  expect(brunchElem).toBeInTheDocument()
  userEvent.click(brunchElem)
  expect(emptyCarElem).not.toBeInTheDocument()  

})
```

---

## Mocker l'API du navigateur <a name="api">

Les navigateurs on des fonctionnalités intérressantes comme : localStorage, cookies, géolocalisation, notifications ....
> Mais lorsque nous exécution nos tests avec jest il sont executer dans Node (avec une simulation du navigateur [JSDOM](https://github.com/jsdom/jsdom)) et malheureusement tout n'est pas pris en charge. c'est écris dans la page de JSDOM. heureusement nous pouvons `mocker` ces implémentations manquantes avec `jest.fn()` et `mockImplementation`. Par exemple pour la géolocalisation.

````javascript
beforeAll(() => {
  window.navigator.geolocation = {
    getCurrentPosition: jest.fn(),
  }
})

window.navigator.geolocation.getCurrentPosition.mockImplementation(() => {
  return {
    coords: {
      latitude: 10,
      longitude: 235,
    },
  }
})
````

Exemple

````javascript
// LoginSubmit
import * as React from 'react'

import LoginForm from './loginForm'
function LoginSubmit() {
  const [dataToSubmit, setDataToSubmit] = React.useState(null)
  const [fetchStatus, setFetchStatus] = React.useState({
    status: 'idle',
    payload: '',
  })
  const [permission, setPermission] = React.useState(null)

  const askPerm = async () => {
    let perm = await window.Notification.requestPermission()
    setPermission(perm)
  }

  // eslint-disable-next-line
  const notifyUser = name => {
    // Vérifions si les autorisations de notification ont déjà été accordées
    if (window.Notification.permission === 'granted') {
      // Si tout va bien, créons une notification
      console.log('indow.Notification.permissio grantedp', askPerm())
      new Notification(`bonjour ${name}`)
    }
  }

  const endpoint = 'https://example.com/api/login'
  const body = dataToSubmit ? JSON.stringify(dataToSubmit) : null
  React.useEffect(() => {
    if (body) {
      askPerm()
      setFetchStatus({status: 'feching', payload: ''})
      fetch(endpoint, {
        method: 'POST',
        body: body,
        headers: {
          'content-type': 'application/json',
        },
      }).then(async response => {
        const data = await response.json()
        if (response.ok) {
          setFetchStatus({status: 'done', payload: data})
          notifyUser(data.username)
        } else {
          setFetchStatus({status: 'error', payload: data})
        }
      })
      // .catch(err => {
      //   setFetchStatus({status: 'done', payload: {username: 'mike'}})
      //   notifyUser("mike")
      // })
      //.catch(err => setFetchStatus({status: 'error', payload: {errorMessage :err.message}}))
      //  .catch(err => setFetchStatus({status: 'error', payload: {errorMessage :"le username est obligatoire"}}))
    }
    // eslint-disable-next-line
  }, [dataToSubmit, body])
  return (
    <div>
      <div>
        {permission === 'granted'
          ? 'Les notifications sont autorisés'
          : 'veuillez autoriser les notifications'}
      </div>
      {fetchStatus.status === 'feching' ? 'chargement...' : null}

      {fetchStatus.status === 'error' ? (
        <div role="alert" style={{color: 'red'}}>
          {fetchStatus.payload.errorMessage}
        </div>
      ) : null}
      {fetchStatus.status === 'done' ? (
        <div>
          Bonjour <strong>{fetchStatus.payload.username}</strong>
        </div>
      ) : (
        <LoginForm onSubmit={formData => setDataToSubmit(formData)} />
      )}
    </div>
  )
}

export default LoginSubmit
````

````javascript
/* eslint-disable no-unused-vars */
import * as React from 'react'
import LoginSubmitNotification from '../../components/loginSubmitNotification'
import LoginSubmit from '../../components/loginSubmit'
import {render, screen, waitForElementToBeRemoved} from '@testing-library/react'
import userEvent from '@testing-library/user-event'
import faker from 'faker'
import mockHandlers from '../../test/mock-handlers'
import {setupServer} from 'msw/node'
import {act} from 'react-dom/test-utils'

const server = setupServer(...mockHandlers)

beforeAll(() => {
  server.listen(
    (window.Notification = {
      requestPermission: jest.fn(),
    }),
  )
})
afterAll(() => server.close())
afterEach(() => server.resetHandlers())

test('affiche un message de permission `granted` de notification" ', async () => {
  act(() => {
    render(<LoginSubmitNotification />)
  })

  const fakePermission = 'granted'

  window.Notification.requestPermission.mockImplementation(() => {
    return fakePermission
  })

  const username = faker.internet.userName()
  const password = faker.internet.password()

  const usernameElement = screen.queryByLabelText(/Nom d'utilisateur :/i)
  const passwordElement = screen.queryByLabelText(/Mot de passe :/i)
  const submitbuttonElement = screen.queryByLabelText('button', {name: /Connexion/i})

  userEvent.type(usernameElement, username)
  userEvent.type(passwordElement, password)

  await act(async () => {
    userEvent.click(await screen.queryByRole("button", { name: "Connexion" }));
  })
    await waitForElementToBeRemoved(() =>
      screen.queryByText(/chargement.../i),
    )
    expect(
      screen.getByText(/Les notifications sont autorisés/i),
    ).toBeInTheDocument()
})
````

---

## Mocker un module <a name="module">

---

## Test componsant avec Jest sans React Testing Library <a name="onlyjest">

````javascript
import * as React from 'react'
import ReactDOM from 'react-dom'
import Hello from '../../components/hello'

  test('Affiche "Bonjour John" et "Merci" lors d\'un click" ', () => {
    const div = document.createElement('div')
    div.classList.add("foo", "bar", "baz")
    document.body.append(div)
  
    ReactDOM.render(<Hello name="John" />, div)
  
    const envoyer = div.querySelector('input')
    const label = div.firstChild.querySelector('div')
  
    expect(label.textContent).toBe(`Bonjour John`)
    envoyer.click()
    expect(label.textContent).toBe(`Merci`)
  })

//<Hello />
// import * as React from 'react'

// function Hello({name}) {
//   const [label, setLabel] = React.useState(`Bonjour ${name}`)
//   return (
//     <div>
//       <div>
//         <div role="status">{label}</div>
//       </div>
//       <input type="button" value="envoyer" onClick={e => setLabel(`Merci`)} />
//     </div>
//   )
// }
// export default Hello
````

Même chose que dessus mais on remplave le `.click()` par un `dispatchEvent`

````javascript
.
.
.
expect(label.textContent).toBe(`Bonjour John`)
  const envoyerClickEvent = new MouseEvent('click', {
    bubbles: true,
    cancelable: true,
    button: 0,
  })
  envoyer.dispatchEvent(envoyerClickEvent)
  expect(label.textContent).toBe(`Merci`)
})
````

---

## Test componsant avec Jest ET React Testing Library <a name="jestrtl">

````javascript
import * as React from 'react'
import {render, fireEvent} from '@testing-library/react'
import Hello from '../../components/hello'

test('Affiche "Bonjour John" et "Merci" lors d\'un click" ', () => {
  const {container} = render(<Hello name="John" />)
  const envoyer = container.querySelector('input')
  const label = container.firstChild.querySelector('div')

  expect(label.textContent).toBe(`Bonjour John`)
  fireEvent.click(envoyer)
  expect(label.textContent).toBe(`Merci`)
})
````

## Test componsant avec Jest ET React Testing Library et l'extension `jest-dom` <a name="jestrtldom">

````javascript
// 🚀 Utilisation de @testing-library/jest-dom
import * as React from 'react'
import Hello from '../../components/hello'
import {render, fireEvent} from '@testing-library/react'

test('Affiche "Bonjour John" et "Merci" lors d\'un click" ', () => {
  const {container} = render(<Hello name="John" />)
  const envoyer = container.querySelector('input')
  const label = container.firstChild.querySelector('div')

  expect(label).toHaveTextContent(`Bonjour John`)
  fireEvent.click(envoyer)
  expect(label).toHaveTextContent(`Merci`)
````

## Test en `boite noire` <a name="boite">

>Le test de la boîte noire, ou test de la boîte opaque, est utilisé en programmation informatique et en génie logiciel pour tester un programme en vérifiant que les sorties obtenues sont bien celles prévues pour des entrées données.

````HTML

 <!-- exemple ici avec un input de type button -->
<input type="button" value="envoyer" onClick={e => setLabel(`Merci`)} />
 <!-- mais si on change l'input par un button les tests seront en erreur -->
<button />

 <!-- Autre exemple avec une div et on récupère le premier enfant -->
 <div> 
  <div></div>
 </div>
 <!-- si on rajouter une div les tests seront en erreur -->
 <div> 
  <div></div>
  <div></div>
 </div>
````

React testing library nous fournit des fonctionnalités intérressantes pour tester le plus possible en boite noire et ne plus s'occuper des détails d'implémentation du code.

Exemple : `getByRole` va récupérer un élément en fonction de son rôle définit dans l'arbre d'accessibilité

````javascript
import * as React from 'react'
import Hello from '../../components/helloreset'
import {render, screen, fireEvent} from '@testing-library/react'

test('Affiche "Bonjour John" et "Merci" lors d\'un click" ', () => {
  render(<Hello name="John" />)

  const envoyer = screen.getByRole('button', {name: /envoyer/i})
  const reset = screen.getByRole('button', {name: /reset/i})
  const label = screen.getByRole('status')

  expect(label).toHaveTextContent(`Bonjour John`)
  fireEvent.click(envoyer)
  expect(label).toHaveTextContent(`Merci`)
  fireEvent.click(reset)
  expect(label).toHaveTextContent(`Bonjour John`)
})

// helloreset
import * as React from 'react'

function HelloReset({name}) {
  const [label, setLabel] = React.useState(`Bonjour ${name}`)
  return (
    <div>
      <div>
        <div role="status">{label}</div>
      </div>
      <input type="button" value="envoyer" onClick={e => setLabel(`Merci`)} />
      <input
        type="button"
        value="reset"
        onClick={e => setLabel(`Bonjour ${name}`)}
      />
    </div>
  )
}
export default HelloReset
````

## `userEvent` de testing library <a name="userevent">

````javascript
import * as React from 'react'
import Hello from '../../components/helloreset'
import {render, screen} from '@testing-library/react'
import userEvent from '@testing-library/user-event'

test('Affiche "Bonjour John" et "Merci" lors d\'un click" ', () => {
  render(<Hello name="John" />)

  const envoyer = screen.getByRole('button', {name: /envoyer/i})
  const reset = screen.getByRole('button', {name: /reset/i})
  const label = screen.getByRole('status')

  expect(label).toHaveTextContent(`Bonjour John`)
  userEvent.click(envoyer)
  expect(label).toHaveTextContent(`Merci`)
  userEvent.click(reset)
  expect(label).toHaveTextContent(`Bonjour John`)
})
````

## Annexes <a name="annexes">

```shell script
npm test -- --coverage
```

````javascript
// si aucun test dans le fichier .todo permet de ne pas générer une erreur en attendant de dev le test
test.todo('Retourne une nombre entier alétoire')
````

````javascript
// A voir ! dans React testing library
// https://subscription.packtpub.com/book/web-development/9781800564459/2/ch02lvl1sec14/using-the-debug-method
screen.debug()
````

````javascript
// erreur typique

 //TestingLibraryElementError: Unable to find an element with the text: /chargement.../i. This could be because the text is broken up by multiple elements. In this case, you can provide a function for your text matcher to make your matcher more flexible.

const usernameElement = screen.getByText(/Nom d'utilisateur :/i)
//remplacer
const usernameElement = screen.queryByLabelText(/Nom d'utilisateur :/i)

````
