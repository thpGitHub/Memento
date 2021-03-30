# Tests avec JavaScript

## Les méthodologies

- TDD Test Driven Development ->
    Ecrire le test avant le code.

- BDD Behavior Driven Development ->
    Programmation piloté par le comportement.
    Méthode agile qui encourage la collaboration (développeurs, responsable qualité, intervenants techniques). Le BDD est la couche fonctionnelle, allant de pair avec le TDD.

- ATTD Acceptance Test Driven Development.

## Assertions

Expression qui doit être évaluée à `true`.
Librairies d'assertions : Chai, Assert, Assert-plus...

## Les Mocks

Librairies de Mockup : Faker, Sinon?js, Mock Jest, Miragejs...

## Tests unitaires avec `JEST`

````shell script
mkdir jest-sample
cd jest-sample
nmp init
npm install --save-dev jest
````

Par convention les noms de fichier de test `nomFichierAtester.test.js`

Il y a 3 étapes afaire dans le fichier de test :

- 1 importer le fichier à tester
- 2 rédaction du test
- 3 assertion

````javascript
// fichier multiplication.js
function multiplication(a, b) {
    return a*b;
}
// export default multiplication; // l'export ES6 ne fonctionne pas avec Jest
module.exports = multiplication;
````

````javascript
// fichier multiplication.test.js
// import multiplication from './multiplication.js'; // l'import ES6 avec jest ne fonctionne pas
const multiplication = require('./multiplication');

test('10 multiplier par 9 egal 90', () => {
    expect(multiplication(9, 10)).toBe(90);
});
````

Deux technique pour executer un test

- Rajouter un script dans le package.json

````json
"scripts": {
    "test": "jest"
  },
````

- Dans un terminal, pour cela il faut installer `JEST` de manière global dans notre ``PATH`

````shell script
npm install jest --global
jest multiplication.test.js
````
