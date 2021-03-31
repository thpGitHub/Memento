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

Par convention les noms de fichier de test :`nomFichierAtester.test.js` ou `nomFichierAtester.spec.js` ou dans un dossier `__tests__`

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

### Deux techniques pour executer un test

- Rajouter un script dans le package.json

````json
"scripts": {
    "test": "jest"
  },
````

Il est possible avec l'option `watch` de relancer les tests automatiquement dès qu'un nouveau fichier de test est créé.

````json
"scripts": {
    "test": "jest --watch"
  },
````

- Dans un terminal, pour cela il faut installer `JEST` de manière global dans notre ``PATH`

````shell script
npm install jest --global
jest multiplication.test.js
````

### Ajouter un fichier de configuration supplémentaire

````shell script
jest --init
# Attention pour que la commande jest soit reconnue il faut installer jest de manière global
````

### Installation de `babel` pour du JS moderne

````shell script
npm i @babel/core babel-jest @babel/preset-env --save-dev
# création du fichier de config a la racine du projet
touch babel.config.js
````

````javascript
// babel.config.js
module.exports = {
  presets: [['@babel/preset-env', {targets: {node: 'current'}}]],
};
````

Cela va nous permettre d'utiliser `ìmport` et `export` dans nos fichiers au lieu de ``require` et `module.exports`

````javascript
function multiplication(a, b) {
    return a*b;
}
// export default multiplication; // l'export ES6 ne fonctionne pas avec Jest
export default multiplication;
````

````javascript
import multiplication from './multiplication';

test('Multiplier des nombres', () => {
    expect(multiplication(9, 10)).toBe(90);
    expect(multiplication(5, 5)).toBe(25);
});
````

 ---

- `describe` : Regroupe des tests en une suite

- `test` `it` : Exécute le test unitaire

- `expect` : Donne accès aux `mathers` (`assert`)

````javascript
//calculHelper.js
let calculAir = function(longeur, largeur) {
    return multiplication(longeur,largeur);
}
let calculAirCarre = function(longeur) {
    return multiplication(longeur, longeur);
}
let multiplication = function(a, b) {
    return a * b;
}

let afficheMessageCalculAir = (a, b) => {
    const air = calculAir(a, b);
    let libelle = `l'air de la surface est de ${air}`;
    if(isNaN(air)) {
        libelle = `l'air ne peut pas être calculée`;
    } 
    return libelle;
}

export {
    calculAir,
    calculAirCarre,
    multiplication,
    afficheMessageCalculAir
}
````

````javascript
//calculHelper.test.js
import { afficheMessageCalculAir } from './calculHelper';

describe('test de la fonctionnalité afficheMessageCalculAir ', () => {
    test('tester le libellé avec des valeurs correctes pour calculAir', () => {
        expect(afficheMessageCalculAir(10, 10)).toContain(100)
    })
    test('tester le libellé avec des valeurs incorrectes pour calculAir', () => {
        expect(afficheMessageCalculAir(10, "toto")).toContain("l'air ne peut pas être calculée")
    })
    test('tester le libellé avec des valeurs vides pour calculAir', () => {
        expect(afficheMessageCalculAir()).toContain("l'air ne peut pas être calculée")
    })
})
````
