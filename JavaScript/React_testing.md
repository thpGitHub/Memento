# React Testing

Avec `create-react-app` `Jest` est déjà installé.

`Jest` recherchera des fichiers de test avec l'une des conventions de dénomination courantes suivantes :

- Fichiers avec `.js` suffixe dans le dossier `__tests__`.
- Fichiers avec `.test.js` suffixe.
- Fichiers avec `.spec.js` suffixe.

Les fichiers `.test.js/` `.spec.js`(ou les dossiers `__tests__`) peuvent être situés à n'importe quelle profondeur sous le `src` dossier de niveau supérieur.

Nous vous recommandons de placer les fichiers (ou dossiers `__tests__`) de test à côté du code qu'ils testent afin que les importations relatives apparaissent plus courtes. Par exemple, si `App.test.js` et `App.js` se trouvent dans le même dossier, le test n'a besoin que de `import App from './App'` au lieu d'un long chemin relatif. La colocalisation permet également de trouver des tests plus rapidement dans des projets plus importants.

## React Testing Library

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

## testing-library/jest-dom

Matchers Jest personnalisés pour tester l'état du DOM.
Cette librairie propose des assertions supplémentaires aux assertions de Jest (qui sont plus génériques). Par exemple si nous voulons savoir si un élément contient un classe css nous pourrions utiliser :

```javascript
expect(deleteButton).toHaveClass('btn-link')
```

- <https://github.com/testing-library/jest-dom>

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
