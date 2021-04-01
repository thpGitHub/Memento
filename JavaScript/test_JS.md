# Tests avec JavaScript

## Les méthodologies

- TDD Test Driven Development ->
    Ecrire le test avant le code.

- BDD Behavior Driven Development ->
    Programmation piloté par le comportement.
    Méthode agile qui encourage la collaboration (développeurs, responsable qualité, intervenants techniques). Le BDD est la couche fonctionnelle, allant de pair avec le TDD.

- ATTD Acceptance Test Driven Development.

## Assertions

Expression qui doit être évaluée à **`true`**.
Librairies d'assertions : Chai, Assert, Assert-plus...

## Les Mocks

Librairies de Mockup : Faker, Sinon?js, Mock Jest, Miragejs...

---

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

- `describe` : Regroupe une suite de tests

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

const isAdmin = (user) => {    
    if(user.role === "admin") {
        return true;
    } else {
        throw new Error('interdit');
    }
}

export {
    calculAir,
    calculAirCarre,
    multiplication,
    afficheMessageCalculAir,
    isAdmin
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

--- 

### Asserts / Matchers

````javascript
import { afficheMessageCalculAir, calculAir, calculAirCarre, isAdmin } from './calculHelper';

//1er test
describe('test de la fonctionnalité afficheMessageCalculAir ', () => {
    test('tester le libellé avec des valeurs correctes pour calculAir', () => {
        expect(afficheMessageCalculAir(10, 10)).toContain(100);
    })
    test('tester le libellé avec des valeurs incorrectes pour calculAir', () => {
        expect(afficheMessageCalculAir(10, "toto")).toContain("l'air ne peut pas être calculée");
    })
    test('tester le libellé avec des valeurs vides pour calculAir', () => {
        expect(afficheMessageCalculAir()).toContain("l'air ne peut pas être calculée");
    })
})

//2eme test
describe('test de la fonctionnalité CalculAir ', () => {
    test('tester des valeurs pour calculAir', () => {
        expect(calculAir(10, 10)).toBe(100);
    })
    test('tester des valeurs pour calculAir', () => {
        expect(calculAir(10, 10)).toBeGreaterThan(0);
    })
    test('tester des valeurs pour calculAir', () => {
        expect(calculAir(10, 10)).toBeGreaterThanOrEqual(100);
    })
    test('tester des valeurs pour calculAir', () => {
        expect(calculAir(10, 10)).not.toBeNaN();
    })
})
//3eme test
describe('test de la fonctionnalité calculAirCarre ', () => {
    test('tester des valeurs pour calculAirCarre', () => {
        expect(calculAirCarre(10)).toBe(100);
    })
    test('tester des valeurs pour calculAirCarre', () => {
        expect(calculAirCarre(10)).toBeGreaterThan(0);
    })
})
//test error
describe('test de la fonctionnalité isAdmin ', () => {
    const userSimple = { role: 'guest' };
    const userAdmin  = { role: 'admin' };

    test('tester isAdmin avec user autre que admin', () => {
        // pour les erreurs il faut cather l'erreur dans une fonction intermédiaire
        function callIsAdmin() {
            isAdmin(userSimple);
        }
        expect(callIsAdmin).toThrowError('interdit');
    })
    test('tester isAdmin avec user admin', () => {
        expect(isAdmin(userAdmin)).toBeTruthy();
    })
})

// toMatchObject
const advancedPermission = {
    domaine: 'toto.com',
    level: 4,
    perms: {
        roles: ['guest', 'reader', 'reviewer'],
        delegated: true,
        method: 'oauth2'
    }
}
const advancedUser = {
    domaine: 'toto.com',
    level: 4,
    perms: {
        roles: ['guest', 'reader', 'reviewer'],
        delegated: true,
        method: expect.stringMatching('saml|oauth|oauth2')
    }
}

describe('test object avancé permission ', () => {
    test('teste de permission', () => {
        expect(advancedPermission).toMatchObject(advancedUser);
    })
})   

// toBeinstanceOf
class User {
    construtor(nom) {
        this.nom = nom;
    }
}

function auth(name) {
    if(typeof name === 'undefined') {
        throw new Error('le nom doit être defini')
    }
    return new User(name);
}

describe('test instance de class ', () => {
    test('tester instance de User', () => {
        expect(new User()).toBeInstanceOf(User);
    })
    test('tester instance de User', () => {
        expect(auth('titi')).toBeInstanceOf(User);
    })
    test('tester instance de User', () => {
        // pour les erreurs il faut cather l'erreur dans une fonction intermédiaire
        function callAuth() {
            auth();
        }
        expect(callAuth).toThrowError('le nom doit être defini');
    })
})   

//arrayContaining

function getRolesA() {
    return ['admin', 'guest'];
}
function getRolesB() {
    return ['admin', 'user'];
}

describe('test sur de array de roles ', () => {
    const attendu = ['admin', 'guest'];
    test('teste array same', () => {
        expect(getRolesA()).toEqual(expect.arrayContaining(attendu));
    })
    test('teste array array not same', () => {
        expect(getRolesB()).not.toEqual(expect.arrayContaining(attendu));
    })
})   
````

---

### Les tests Asynchrones

````javascript
// Asynchrone avec les callbacks en utilsant done()

function fetchAPI(callback) {
    setTimeout(() => {
        callback("{api:ok}");
    },3000)
}

describe('test de  fetch API async ', () => {
        test('testee fetchAPI', (done) => {
            function callback(data) {
                try {
                    expect(data).toBe("{api:ok}");
                    done();
                } catch (error) {
                    done(error)                    ;
                }
            }
            fetchAPI(callback);
        })
})   

// Asynchrone avec les promises

function fetchApiPromise() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve("{api:ok}");
        },3000)
    })
}

test('test de promise', () => {
    return fetchApiPromise().then(data => {
        expect(data).toBe("{api:ok}");
    })
})
````

---

### Jest Mock

````javascript
// Mock

function forEach(items, callback) {
    for(let i = 0; i<items.length; i++) {
        callback(items[i]);
    }
}

test('test de la fonction forEach avec mock', () => {
    const mockCallBack = jest.fn(x => 'salut '+ x);
    forEach(['toto', 'titi'], mockCallBack);

    expect(mockCallBack.mock.calls.length).toBe(2),
    expect(mockCallBack.mock.calls[0][0]).toBe('toto'),
    expect(mockCallBack.mock.calls[1][0]).toBe('titi'),
    expect(mockCallBack.mock.results[0].value).toBe('salut toto'),
    expect(mockCallBack.mock.results[1].value).toBe('salut titi')
})
````

---

### Tester les composants React
