# React Testing

## React Testing Library

- <https://github.com/testing-library/react-testing-library>

```shell script
npm install --save-dev @testing-library/react
```

```jsx
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

Matchers Jest personnalisés pour tester l'état du DOM

- <https://github.com/testing-library/jest-dom>

```shell script
npm install --save-dev @testing-library/jest-dom
```