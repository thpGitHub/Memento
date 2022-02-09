# React Testing

## React Testing Library

- <https://github.com/testing-library/react-testing-library>

```shell script
npm install --save-dev @testing-library/react
```

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

```shell script
npm install --save-dev @testing-library/jest-dom
```

```javascript
// Import @testing-library/jest-dom once (for instance in your tests setup file) and you're good to go:
// In your own jest-setup.js (or any other name) at the root of the folder src/
import '@testing-library/jest-dom'
```

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
