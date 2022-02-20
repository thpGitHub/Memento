# React Testing

## `Jest`

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

Par convention on remplace `test()` par `it()`

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

## React Testing Library

Avec `create-react-app` `React Testing Library` est déjà installé.

Est une bibliothèque qui donne accès à davantage d'outils permettant de tester des composants. Pour tester nos composants, il faudra donc faire un `render`, vérifier le DOM généré, et le comparer avec ce qui était attendu. Cette bibliothèque ne remplace pas Jest, au contraire, elle est complémentaire à Jest. `React Test Library` nous permet de nous concentrer sur le DOM, en le recréant, en permettant de simuler des interactions et de vérifier ce qui est rendu. Cela nous aide à nous mettre dans la peau de nos utilisateurs, et à anticiper ce qu'ils verront.

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

## testing-library/jest-dom

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

## Annexes

```shell script
npm test -- --coverage
```
